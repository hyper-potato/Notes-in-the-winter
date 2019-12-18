[ruder.io](https://ruder.io/optimizing-gradient-descent/ "An overview of gradient descent optimization algorithms")

https://ruder.io/optimizing-gradient-descent/

# An overview of gradient descent optimization algorithms

This post explores how many of the most popular gradient-based optimization algorithms actually work.

Gradient descent is one of the most popular algorithms to perform optimization and by far the most common way to optimize neural networks. At the same time, every state-of-the-art Deep Learning library contains implementations of various algorithms to optimize gradient descent (e.g. [lasagne's][9], [caffe's][10], and [keras'][11] documentation). These algorithms, however, are often used as black-box optimizers, as practical explanations of their strengths and weaknesses are hard to come by.

This blog post aims at providing you with intuitions towards the behavior of different algorithms for optimizing gradient descent that will help you put them to use. We are first going to look at the different variants of gradient descent. We will then briefly summarize challenges during training. Subsequently, we will introduce the most common optimization algorithms by showing their motivation to resolve these challenges and how this leads to the derivation of their update rules. We will also take a short look at algorithms and architectures to optimize gradient descent in a parallel and distributed setting. Finally, we will consider additional strategies that are helpful for optimizing gradient descent.

Gradient descent is a way to minimize an objective function ($J(\theta)$) parameterized by a model's parameters ($\theta \in \mathbb{R}^d$) by updating the parameters in the opposite direction of the gradient of the objective function ($\nabla_\theta J(\theta)$) w.r.t. to the parameters. The learning rate ($\eta$) determines the size of the steps we take to reach a (local) minimum. In other words, we follow the direction of the slope of the surface created by the objective function downhill until we reach a valley. If you are unfamiliar with gradient descent, you can find a good introduction on optimizing neural networks [here][12].

# Gradient descent variants

There are three variants of gradient descent, which differ in how much data we use to compute the gradient of the objective function. Depending on the amount of data, we make a trade-off between the accuracy of the parameter update and the time it takes to perform an update.

## Batch gradient descent

Vanilla gradient descent, aka batch gradient descent, computes the gradient of the cost function w.r.t. to the parameters (theta) for the entire training dataset:

$\theta = \theta - \eta \cdot \nabla_\theta J( \theta)$

As we need to calculate the gradients for the whole dataset to perform just _one_ update, batch gradient descent can be very slow and is intractable for datasets that don't fit in memory. Batch gradient descent also doesn't allow us to update our model _online_, i.e. with new examples on-the-fly.

In code, batch gradient descent looks something like this:

```python
for i in range(nb_epochs):
  params_grad = evaluate_gradient(loss_function, data, params)
  params = params - learning_rate * params_grad
```

For a pre-defined number of epochs, we first compute the gradient vector `params_grad` of the loss function for the whole dataset w.r.t. our parameter vector `params`. Note that state-of-the-art deep learning libraries provide automatic differentiation that efficiently computes the gradient w.r.t. some parameters. If you derive the gradients yourself, then gradient checking is a good idea. (See [here][13] for some great tips on how to check gradients properly.)

We then update our parameters in the opposite direction of the gradients with the learning rate determining how big of an update we perform. Batch gradient descent is guaranteed to converge to the global minimum for convex error surfaces and to a local minimum for non-convex surfaces.

## Stochastic gradient descent

Stochastic gradient descent (SGD) in contrast performs a parameter update for _each_ training example $x^{(i)}$ and label $y^{(i)}$:

$\theta = \theta - \eta \cdot \nabla_\theta J( \theta; x^{(i)}; y^{(i)})$

Batch gradient descent performs redundant computations for large datasets, as it recomputes gradients for similar examples before each parameter update. SGD does away with this redundancy by performing one update at a time. It is therefore usually much faster and can also be used to learn online. SGD performs frequent updates with a high variance that cause the objective function to fluctuate heavily as in Image 1.

<img src='https://ruder.io/content/images/2016/09/sgd_fluctuation.png'>

Image 1: SGD fluctuation (Source: [Wikipedia][15])

While batch gradient descent converges to the minimum of the basin the parameters are placed in, SGD's fluctuation, on the one hand, enables it to jump to new and potentially better local minima. On the other hand, this ultimately complicates convergence to the exact minimum, as SGD will keep overshooting. However, it has been shown that when we slowly decrease the learning rate, SGD shows the same convergence behaviour as batch gradient descent, almost certainly converging to a local or the global minimum for non-convex and convex optimization respectively. Its code fragment simply adds a loop over the training examples and evaluates the gradient w.r.t. each example. Note that we shuffle the training data at every epoch as explained in [this section][16].

```python
for i in range(nb_epochs):
  np.random.shuffle(data)
  for example in data:
    params_grad = evaluate_gradient(loss_function, example, params)
    params = params - learning_rate * params_grad
```

## Mini-batch gradient descent

Mini-batch gradient descent finally takes the best of both worlds and performs an update for every mini-batch of n training examples:

$\theta = \theta - \eta \cdot \nabla_\theta J( \theta; x^{(i:i+n)}; y^{(i:i+n)})$

This way, it _a)_ reduces the variance of the parameter updates, which can lead to more stable convergence; and _b)_ can make use of highly optimized matrix optimizations common to state-of-the-art deep learning libraries that make computing the gradient w.r.t. a mini-batch very efficient. Common mini-batch sizes range between 50 and 256, but can vary for different applications. Mini-batch gradient descent is typically the algorithm of choice when training a neural network and the term SGD usually is employed also when mini-batches are used. Note: In modifications of SGD in the rest of this post, we leave out the parameters $x^{(i:i+n)}; y^{(i:i+n)}$ for simplicity.

In code, instead of iterating over examples, we now iterate over mini-batches of size 50:

```python
for i in range(nb_epochs):
  np.random.shuffle(data)
  for batch in get_batches(data, batch_size=50):
    params_grad = evaluate_gradient(loss_function, batch, params)
    params = params - learning_rate * params_grad
```

# Challenges

Vanilla mini-batch gradient descent, however, does not guarantee good convergence, but offers a few challenges that need to be addressed:

- Choosing a proper learning rate can be difficult. A learning rate that is too small leads to painfully slow convergence, while a learning rate that is too large can hinder convergence and cause the loss function to fluctuate around the minimum or even to diverge.

- Learning rate schedules try to adjust the learning rate during training by e.g. annealing, i.e. reducing the learning rate according to a pre-defined schedule or when the change in objective between epochs falls below a threshold. These schedules and thresholds, however, have to be defined in advance and are thus unable to adapt to a dataset's characteristics [][18][2].

- Additionally, the same learning rate applies to all parameter updates. If our data is sparse and our features have very different frequencies, we might not want to update all of them to the same extent, but perform a larger update for rarely occurring features.

- Another key challenge of minimizing highly non-convex error functions common for neural networks is avoiding getting trapped in their numerous suboptimal local minima. Dauphin et al. [][19][3] argue that the difficulty arises in fact not from local minima but from saddle points, i.e. points where one dimension slopes up and another slopes down. These saddle points are usually surrounded by a plateau of the same error, which makes it notoriously hard for SGD to escape, as the gradient is close to zero in all dimensions.

# Gradient descent optimization algorithms

In the following, we will outline some algorithms that are widely used by the deep learning community to deal with the aforementioned challenges. We will not discuss algorithms that are infeasible to compute in practice for high-dimensional data sets, e.g. second-order methods such as [Newton's method][20].

## Momentum

SGD has trouble navigating ravines, i.e. areas where the surface curves much more steeply in one dimension than in another [][21][4], which are common around local optima. In these scenarios, SGD oscillates across the slopes of the ravine while only making hesitant progress along the bottom towards the local optimum as in Image 2.



![](https://ruder.io/content/images/2015/12/without_momentum.gif)

 

Image 2: SGD without momentum 

 ![](https://ruder.io/content/images/2015/12/with_momentum.gif)

Image 3: SGD with momentum



Momentum [][24][5] is a method that helps accelerate SGD in the relevant direction and dampens oscillations as can be seen in Image 3\. It does this by adding a fraction (gamma) of the update vector of the past time step to the current update vector:

$\begin{align}  \begin{split}  v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta) \\  \theta &= \theta - v_t  \end{split}  \end{align}$

Note: Some implementations exchange the signs in the equations. The momentum term (gamma) is usually set to 0.9 or a similar value.

Essentially, when using momentum, we push a ball down a hill. The ball accumulates momentum as it rolls downhill, becoming faster and faster on the way (until it reaches its terminal velocity if there is air resistance, i.e. $\gamma < 1$). The same thing happens to our parameter updates: The momentum term increases for dimensions whose gradients point in the same directions and reduces updates for dimensions whose gradients change directions. As a result, we gain faster convergence and reduced oscillation.

# Nesterov accelerated gradient

However, a ball that rolls down a hill, blindly following the slope, is highly unsatisfactory. We'd like to have a smarter ball, a ball that has a notion of where it is going so that it knows to slow down before the hill slopes up again.

Nesterov accelerated gradient (NAG) [][25][6] is a way to give our momentum term this kind of prescience. We know that we will use our momentum term  $\gamma v_{t-1}$ to move the parameters $\theta$. Computing $\theta - \gamma v\_{t-1}$ thus gives us an approximation of the next position of the parameters (the gradient is missing for the full update), a rough idea where our parameters are going to be. We can now effectively look ahead by calculating the gradient not w.r.t. to our current parameters (theta) but w.r.t. the approximate future position of our parameters:

$\begin{align}  \begin{split}  v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta - \gamma v_{t-1} ) \\  \theta &= \theta - v_t  \end{split}  \end{align}$

Again, we set the momentum term $\gamma$ to a value of around 0.9\. While Momentum first computes the current gradient (small blue vector in Image 4) and then takes a big jump in the direction of the updated accumulated gradient (big blue vector), NAG first makes a big jump in the direction of the previous accumulated gradient (brown vector), measures the gradient and then makes a correction (red vector), which results in the complete NAG update (green vector). This anticipatory update prevents us from going too fast and results in increased responsiveness, which has significantly increased the performance of RNNs on a number of tasks [][26][7].

![](https://ruder.io/content/images/2016/09/nesterov_update_vector.png)

Image 4: Nesterov update (Source: [G. Hinton's lecture 6c][28])

Refer to [here][13] for another explanation about the intuitions behind NAG, while Ilya Sutskever gives a more detailed overview in his PhD thesis [][29][8].

Now that we are able to adapt our updates to the slope of our error function and speed up SGD in turn, we would also like to adapt our updates to each individual parameter to perform larger or smaller updates depending on their importance.

## Adagrad

Adagrad [][30][9] is an algorithm for gradient-based optimization that does just this: It adapts the learning rate to the parameters, performing smaller updates (i.e. low learning rates) for parameters associated with frequently occurring features, and larger updates (i.e. high learning rates) for parameters associated with infrequent features. For this reason, it is well-suited for dealing with sparse data. Dean et al. [][31][10] have found that Adagrad greatly improved the robustness of SGD and used it for training large-scale neural nets at Google, which -- among other things -- learned to [recognize cats in Youtube videos][32]. Moreover, Pennington et al. [][33][11] used Adagrad to train GloVe word embeddings, as infrequent words require much larger updates than frequent ones.

Previously, we performed an update for all parameters $\theta$ at once as every parameter $\theta_i$ used the same learning rate $\eta$. As Adagrad uses a different learning rate for every parameter $\theta_i$  at every time stept, we first show Adagrad's per-parameter update, which we then vectorize. For brevity, we use $g_{t}$ to denote the gradient at time step t. $g_{t, i}$ is then the partial derivative of the objective function w.r.t. to the parameter $\theta_i$ at time step (t):

$g_{t, i} = \nabla_\theta J( \theta_{t, i} )$

The SGD update for every parameter $\theta_i$ at each time step t then becomes:

$\theta_{t+1, i} = \theta_{t, i} - \eta \cdot g_{t, i}$

In its update rule, Adagrad modifies the general learning rate $\eta$ at each time step t for every parameter $\theta_i$based on the past gradients that have been computed for $\theta_i$:

$\theta_{t+1, i} = \theta_{t, i} - \dfrac{\eta}{\sqrt{G_{t, ii} + \epsilon}} \cdot g_{t, i}$

$G_{t} \in \mathbb{R}^{d \times d}$ here is a diagonal matrix where each diagonal element i, i is the sum of the squares of the gradients w.r.t. $\theta_i$ up to time step t [][34][12], while $\epsilon$ is a smoothing term that avoids division by zero (usually on the order of 1e-8). Interestingly, without the square root operation, the algorithm performs much worse.

As $G_{t}$ contains the sum of the squares of the past gradients w.r.t. to all parameters $\theta$ along its diagonal, we can now vectorize our implementation by performing a matrix-vector product $\odot$ between $G_{t}$ and $g_{t}$:

$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{G_{t} + \epsilon}} \odot g_{t}$

One of Adagrad's main benefits is that it eliminates the need to manually tune the learning rate. Most implementations use a default value of 0.01 and leave it at that.

Adagrad's main weakness is its accumulation of the squared gradients in the denominator: Since every added term is positive, the accumulated sum keeps growing during training. This in turn causes the learning rate to shrink and eventually become infinitesimally small, at which point the algorithm is no longer able to acquire additional knowledge. The following algorithms aim to resolve this flaw.

## Adadelta

Adadelta [][35][13] is an extension of Adagrad that seeks to reduce its aggressive, monotonically decreasing learning rate. Instead of accumulating all past squared gradients, Adadelta restricts the window of accumulated past gradients to some fixed size w.

Instead of inefficiently storing w previous squared gradients, the sum of gradients is recursively defined as a decaying average of all past squared gradients. The running average $E[g^2]_t$ at time step t then depends (as a fraction $\gamma$ similarly to the Momentum term) only on the previous average and the current gradient:

$E[g^2]_t = \gamma E[g^2]_{t-1} + (1 - \gamma) g^2_t$

We set $\gamma$ to a similar value as the momentum term, around 0.9\. For clarity, we now rewrite our vanilla SGD update in terms of the parameter update vector $\Delta \theta_t$:

$\begin{align}  \begin{split}  \Delta \theta_t &= - \eta \cdot g_{t, i} \\  \theta_{t+1} &= \theta_t + \Delta \theta_t \end{split}  \end{align}$

The parameter update vector of Adagrad that we derived previously thus takes the form:

$\Delta \theta_t = - \dfrac{\eta}{\sqrt{G_{t} + \epsilon}} \odot g_{t}$



We now simply replace the diagonal matrix $G_{t}$ with the decaying average over past squared gradients $E[g^2]_t$:

$\Delta \theta_t = - \dfrac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t}$

As the denominator is just the root mean squared RMS error criterion of the gradient, we can replace it with the criterion short-hand:

$\Delta \theta_t = - \dfrac{\eta}{RMS[g]_{t}} g_t$

The authors note that the units in this update (as well as in SGD, Momentum, or Adagrad) do not match, i.e. the update should have the same hypothetical units as the parameter. To realize this, they first define another exponentially decaying average, this time not of squared gradients but of squared parameter updates:

$E[\Delta \theta^2]_t = \gamma E[\Delta \theta^2]_{t-1} + (1 - \gamma) \Delta \theta^2_t$

The root mean squared error of parameter updates is thus:

$RMS[\Delta \theta]_{t} = \sqrt{E[\Delta \theta^2]_t + \epsilon}$

Since $RMS[\Delta \theta]_{t}$ is unknown, we approximate it with the RMS of parameter updates until the previous time step. Replacing the learning rate ($\eta$) in the previous update rule with $RMS[\Delta \theta]_{t-1}$  finally yields the Adadelta update rule:

$\begin{align}  \begin{split}  \Delta \theta_t &= - \dfrac{RMS[\Delta \theta]_{t-1}}{RMS[g]_{t}} g_{t} \\  \theta_{t+1} &= \theta_t + \Delta \theta_t  \end{split}  \end{align}$

With Adadelta, we do not even need to set a default learning rate, as it has been eliminated from the update rule.

## RMSprop

RMSprop is an unpublished, adaptive learning rate method proposed by Geoff Hinton in [Lecture 6e of his Coursera Class][28].

RMSprop and Adadelta have both been developed independently around the same time stemming from the need to resolve Adagrad's radically diminishing learning rates. RMSprop in fact is identical to the first update vector of Adadelta that we derived above:

$\begin{align}  \begin{split}  E[g^2]_t &= 0.9 E[g^2]_{t-1} + 0.1 g^2_t \\  \theta_{t+1} &= \theta_{t} - \dfrac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t}  \end{split}  \end{align}$

RMSprop as well divides the learning rate by an exponentially decaying average of squared gradients. Hinton suggests $\gamma$ to be set to 0.9, while a good default value for the learning rate $\eta$ is 0.001.

## Adam

Adaptive Moment Estimation (Adam) [][36][14] is another method that computes adaptive learning rates for each parameter. In addition to storing an exponentially decaying average of past squared gradients $v_t$like Adadelta and RMSprop, Adam also keeps an exponentially decaying average of past gradients $m_t$, similar to momentum. Whereas momentum can be seen as a ball running down a slope, Adam behaves like a heavy ball with friction, which thus prefers flat minima in the error surface [][37][15]. We compute the decaying averages of past and past squared gradients $m_t$ and $v_t$respectively as follows:

$\begin{align}  \begin{split}  m_t &= \beta_1 m_{t-1} + (1 - \beta_1) g_t \\  v_t &= \beta_2 v_{t-1} + (1 - \beta_2) g_t^2  \end{split}  \end{align}$

$m_t$ and $v_t$are estimates of the first moment (the mean) and the second moment (the uncentered variance) of the gradients respectively, hence the name of the method. As $m_t$ and$v_t$are initialized as vectors of 0's, the authors of Adam observe that they are biased towards zero, especially during the initial time steps, and especially when the decay rates are small (i.e. $\beta_1$ and $\beta_2$ are close to 1).

They counteract these biases by computing bias-corrected first and second moment estimates:

$\begin{align}  \begin{split}  \hat{m}_t &= \dfrac{m_t}{1 - \beta^t_1} \\  \hat{v}_t &= \dfrac{v_t}{1 - \beta^t_2} \end{split}  \end{align}$

They then use these to update the parameters just as we have seen in Adadelta and RMSprop, which yields the Adam update rule:

$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$

The authors propose default values of 0.9 for $\beta_1$, 0.999 for $\beta_2$, and $10^{-8}$ for $\epsilon$. They show empirically that Adam works well in practice and compares favorably to other adaptive learning-method algorithms.

## AdaMax

The $v_t$ factor in the Adam update rule scales the gradient inversely proportionally to the $\ell_2$ norm of the past gradients (via the $v_{t-1}$ term) and current gradient $|g_t|^2$:

$v_t = \beta_2 v_{t-1} + (1 - \beta_2) |g_t|^2$

We can generalize this update to the $\ell_p$ norm. Note that Kingma and Ba also parameterize $\beta_2$ as $\beta^p_2$:

$v_t = \beta_2^p v_{t-1} + (1 - \beta_2^p) |g_t|^p$

Norms for large p values generally become numerically unstable, which is why $\ell_1$  and $\ell_2$ norms are most common in practice. However, $\ell_\infty$also generally exhibits stable behavior. For this reason, the authors propose AdaMax (Kingma and Ba, 2015) and show that $v_t$ with $\ell_\infty$ converges to the following more stable value. To avoid confusion with Adam, we use $u_t$ to denote the infinity norm-constrained $v_t$:

$\begin{align}  \begin{split}  u_t &= \beta_2^\infty v_{t-1} + (1 - \beta_2^\infty) |g_t|^\infty\\  & = \max(\beta_2 \cdot v_{t-1}, |g_t|)  \end{split}  \end{align}$

We can now plug this into the Adam update equation by replacing $\sqrt{\hat{v}_t} + \epsilon$ with $u_t$ to obtain the AdaMax update rule:

$\theta_{t+1} = \theta_{t} - d\frac{\eta}{u_t} \hat{m}_t$

Note that as $u_t$  relies on the (max) operation, it is not as suggestible to bias towards zero as $m_t$ and $v_t$  in Adam, which is why we do not need to compute a bias correction for $u_t$ . Good default values are again $\eta = 0.002$, $\beta_1 = 0.9$, and $\beta_2 = 0.999$.

## Nadam

As we have seen before, Adam can be viewed as a combination of RMSprop and momentum: RMSprop contributes the exponentially decaying average of past squared gradients $v_t$, while momentum accounts for the exponentially decaying average of past gradients  $m _t$. We have also seen that Nesterov accelerated gradient (NAG) is superior to vanilla momentum.

Nadam (Nesterov-accelerated Adaptive Moment Estimation) [][38][16] thus combines Adam and NAG. In order to incorporate NAG into Adam, we need to modify its momentum term  $m _t$.

First, let us recall the momentum update rule using our current notation :

$\begin{align}  \begin{split}  g_t &= \nabla_{\theta_t}J(\theta_t)\\  m_t &= \gamma m_{t-1} + \eta g_t\\  \theta_{t+1} &= \theta_t - m_t  \end{split}  \end{align}$

where J is our objective function, $\gamma$ is the momentum decay term, and $\eta$ is our step size. Expanding the third equation above yields:

$\theta_{t+1} = \theta_t - ( \gamma m_{t-1} + \eta g_t)$

This demonstrates again that momentum involves taking a step in the direction of the previous momentum vector and a step in the direction of the current gradient.

NAG then allows us to perform a more accurate step in the gradient direction by updating the parameters with the momentum step _before_ computing the gradient. We thus only need to modify the gradient $g_t$ to arrive at NAG:

$\begin{align}  \begin{split}  g_t &= \nabla_{\theta_t}J(\theta_t - \gamma m_{t-1})\\  m_t &= \gamma m_{t-1} + \eta g_t\\  \theta_{t+1} &= \theta_t - m_t  \end{split}  \end{align}$

Dozat proposes to modify NAG the following way: Rather than applying the momentum step twice -- one time for updating the gradient $g_t$ and a second time for updating the parameters $\theta_{t+1}$ -- we now apply the look-ahead momentum vector directly to update the current parameters:

$\begin{align}  \begin{split}  g_t &= \nabla_{\theta_t}J(\theta_t)\\  m_t &= \gamma m_{t-1} + \eta g_t\\  \theta_{t+1} &= \theta_t - (\gamma m_t + \eta g_t)  \end{split}  \end{align}$

Notice that rather than utilizing the previous momentum vector $m_{t-1}$ as in the equation of the expanded momentum update rule above, we now use the current momentum vector $m_{t}$ to look ahead. In order to add Nesterov momentum to Adam, we can thus similarly replace the previous momentum vector with the current momentum vector. First, recall that the Adam update rule is the following (note that we do not need to modify $\hat{v}_t$):

$\begin{align}  \begin{split}  m_t &= \beta_1 m_{t-1} + (1 - \beta_1) g_t\\  \hat{m}_t & = \frac{m_t}{1 - \beta^t_1}\\  \theta_{t+1} &= \theta_{t} - \frac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t  \end{split}  \end{align}$

Expanding the second equation with the definitions of $\hat{m}_t$ and $m_t$ in turn gives us:

$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} (\dfrac{\beta_1 m_{t-1}}{1 - \beta^t_1} + \dfrac{(1 - \beta_1) g_t}{1 - \beta^t_1})$

Note that $\dfrac{\beta_1 m_{t-1}}{1 - \beta^t_1}$ is just the bias-corrected estimate of the momentum vector of the previous time step. We can thus replace it with $\hat{m}_{t-1}$:

$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} (\beta_1 \hat{m}_{t-1} + \dfrac{(1 - \beta_1) g_t}{1 - \beta^t_1})$

Note that for simplicity, we ignore that the denominator is $1 - \beta^{t}_1$ and not $1 - \beta^{t-1}_1$ as we will replace the denominator in the next step anyway. This equation again looks very similar to our expanded momentum update rule above. We can now add Nesterov momentum just as we did previously by simply replacing this bias-corrected estimate of the momentum vector of the previous time step $\hat{m}_{t-1}$ with the bias-corrected estimate of the current momentum vector $\hat{m}_t$, which gives us the Nadam update rule:

$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} (\beta_1 \hat{m}_t + \dfrac{(1 - \beta_1) g_t}{1 - \beta^t_1})$

## AMSGrad

As adaptive learning rate methods have become the norm in training neural networks, practitioners noticed that in some cases, e.g. for object recognition [][39][17] or machine translation [][40][18] they fail to converge to an optimal solution and are outperformed by SGD with momentum.

Reddi et al. (2018) [][41][19] formalize this issue and pinpoint the exponential moving average of past squared gradients as a reason for the poor generalization behaviour of adaptive learning rate methods. Recall that the introduction of the exponential average was well-motivated: It should prevent the learning rates to become infinitesimally small as training progresses, the key flaw of the Adagrad algorithm. However, this short-term memory of the gradients becomes an obstacle in other scenarios.

In settings where Adam converges to a suboptimal solution, it has been observed that some minibatches provide large and informative gradients, but as these minibatches only occur rarely, exponential averaging diminishes their influence, which leads to poor convergence. The authors provide an example for a simple convex optimization problem where the same behaviour can be observed for Adam.

To fix this behaviour, the authors propose a new algorithm, AMSGrad that uses the maximum of past squared gradients $v_t$ rather than the exponential average to update the parameters. $v_t$  is defined the same as in Adam above:

$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$

Instead of using $v_t$  (or its bias-corrected version $\hat{v}_t$) directly, we now employ the previous ${v}_{t-1}$if it is larger than the current one:

$\hat{v}_t = \text{max}(\hat{v}_{t-1}, v_t)$

This way, AMSGrad results in a non-increasing step size, which avoids the problems suffered by Adam. For simplicity, the authors also remove the debiasing step that we have seen in Adam. The full AMSGrad update without bias-corrected estimates can be seen below:

$\begin{align}  \begin{split}  m_t &= \beta_1 m_{t-1} + (1 - \beta_1) g_t \\  v_t &= \beta_2 v_{t-1} + (1 - \beta_2) g_t^2\\  \hat{v}_t &= \text{max}(\hat{v}_{t-1}, v_t) \\  \theta_{t+1} &= \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} m_t  \end{split}  \end{align}$

The authors observe improved performance compared to Adam on small datasets and on CIFAR-10\. [Other experiments][42], however, show similar or worse performance than Adam. It remains to be seen whether AMSGrad is able to consistently outperform Adam in practice. For more information about recent advances in Deep Learning optimization, refer to [this blog post][43].

## Visualization of algorithms

The following two animations (Image credit: [Alec Radford][44]) provide some intuitions towards the optimization behaviour of most of the presented optimization methods. Also have a look [here][13] for a description of the same images by Karpathy and another concise overview of the algorithms discussed.

In Image 5, we see their behaviour on the contours of a loss surface ([the Beale function][45]) over time. Note that Adagrad, Adadelta, and RMSprop almost immediately head off in the right direction and converge similarly fast, while Momentum and NAG are led off-track, evoking the image of a ball rolling down the hill. NAG, however, is quickly able to correct its course due to its increased responsiveness by looking ahead and heads to the minimum.

Image 6 shows the behaviour of the algorithms at a saddle point, i.e. a point where one dimension has a positive slope, while the other dimension has a negative slope, which pose a difficulty for SGD as we mentioned before. Notice here that SGD, Momentum, and NAG find it difficulty to break symmetry, although the two latter eventually manage to escape the saddle point, while Adagrad, RMSprop, and Adadelta quickly head down the negative slope.

 ![][46] 

Image 5: SGD optimization on loss surface contours![][47] 

Image 6: SGD optimization on saddle point

As we can see, the adaptive learning-rate methods, i.e. Adagrad, Adadelta, RMSprop, and Adam are most suitable and provide the best convergence for these scenarios.

Note: If you are interested in visualizing these or other optimization algorithms, refer to [this useful tutorial][48].

## Which optimizer to use?

So, which optimizer should you now use? If your input data is sparse, then you likely achieve the best results using one of the adaptive learning-rate methods. An additional benefit is that you won't need to tune the learning rate but likely achieve the best results with the default value.

In summary, RMSprop is an extension of Adagrad that deals with its radically diminishing learning rates. It is identical to Adadelta, except that Adadelta uses the RMS of parameter updates in the numinator update rule. Adam, finally, adds bias-correction and momentum to RMSprop. Insofar, RMSprop, Adadelta, and Adam are very similar algorithms that do well in similar circumstances. Kingma et al. [[14:1]][36] show that its bias-correction helps Adam slightly outperform RMSprop towards the end of optimization as gradients become sparser. Insofar, Adam might be the best overall choice.

Interestingly, many recent papers use vanilla SGD without momentum and a simple learning rate annealing schedule. As has been shown, SGD usually achieves to find a minimum, but it might take significantly longer than with some of the optimizers, is much more reliant on a robust initialization and annealing schedule, and may get stuck in saddle points rather than local minima. Consequently, if you care about fast convergence and train a deep or complex neural network, you should choose one of the adaptive learning rate methods.

# Parallelizing and distributing SGD

Given the ubiquity of large-scale data solutions and the availability of low-commodity clusters, distributing SGD to speed it up further is an obvious choice. SGD by itself is inherently sequential: Step-by-step, we progress further towards the minimum. Running it provides good convergence but can be slow particularly on large datasets. In contrast, running SGD asynchronously is faster, but suboptimal communication between workers can lead to poor convergence. Additionally, we can also parallelize SGD on one machine without the need for a large computing cluster. The following are algorithms and architectures that have been proposed to optimize parallelized and distributed SGD.

## Hogwild!

Niu et al. [][49][20] introduce an update scheme called Hogwild! that allows performing SGD updates in parallel on CPUs. Processors are allowed to access shared memory without locking the parameters. This only works if the input data is sparse, as each update will only modify a fraction of all parameters. They show that in this case, the update scheme achieves almost an optimal rate of convergence, as it is unlikely that processors will overwrite useful information.

## Downpour SGD

Downpour SGD is an asynchronous variant of SGD that was used by Dean et al. [[10:1]][31] in their DistBelief framework (predecessor to TensorFlow) at Google. It runs multiple replicas of a model in parallel on subsets of the training data. These models send their updates to a parameter server, which is split across many machines. Each machine is responsible for storing and updating a fraction of the model's parameters. However, as replicas don't communicate with each other e.g. by sharing weights or updates, their parameters are continuously at risk of diverging, hindering convergence.

## Delay-tolerant Algorithms for SGD

McMahan and Streeter [][50][21] extend AdaGrad to the parallel setting by developing delay-tolerant algorithms that not only adapt to past gradients, but also to the update delays. This has been shown to work well in practice.

## TensorFlow

[TensorFlow][51] [][52][22] is Google's recently open-sourced framework for the implementation and deployment of large-scale machine learning models. It is based on their experience with DistBelief and is already used internally to perform computations on a large range of mobile devices as well as on large-scale distributed systems. For distributed execution, a computation graph is split into a subgraph for every device and communication takes place using Send/Receive node pairs. However, the open source version of TensorFlow currently does not support distributed functionality (see [here][53]). Update 13.04.16: A distributed version of TensorFlow has [been released][54].

## Elastic Averaging SGD

Zhang et al. [][55][23] propose Elastic Averaging SGD (EASGD), which links the parameters of the workers of asynchronous SGD with an elastic force, i.e. a center variable stored by the parameter server. This allows the local variables to fluctuate further from the center variable, which in theory allows for more exploration of the parameter space. They show empirically that this increased capacity for exploration leads to improved performance by finding new local optima.

# Additional strategies for optimizing SGD

Finally, we introduce additional strategies that can be used alongside any of the previously mentioned algorithms to further improve the performance of SGD. For a great overview of some other common tricks, refer to [][56][24].

## Shuffling and Curriculum Learning

Generally, we want to avoid providing the training examples in a meaningful order to our model as this may bias the optimization algorithm. Consequently, it is often a good idea to shuffle the training data after every epoch.

On the other hand, for some cases where we aim to solve progressively harder problems, supplying the training examples in a meaningful order may actually lead to improved performance and better convergence. The method for establishing this meaningful order is called Curriculum Learning [][57][25].

Zaremba and Sutskever [][58][26] were only able to train LSTMs to evaluate simple programs using Curriculum Learning and show that a combined or mixed strategy is better than the naive one, which sorts examples by increasing difficulty.

## Batch normalization

To facilitate learning, we typically normalize the initial values of our parameters by initializing them with zero mean and unit variance. As training progresses and we update parameters to different extents, we lose this normalization, which slows down training and amplifies changes as the network becomes deeper.

Batch normalization [][59][27] reestablishes these normalizations for every mini-batch and changes are back-propagated through the operation as well. By making normalization part of the model architecture, we are able to use higher learning rates and pay less attention to the initialization parameters. Batch normalization additionally acts as a regularizer, reducing (and sometimes even eliminating) the need for Dropout.

## Early stopping

According to Geoff Hinton: "_Early stopping (is) beautiful free lunch_" ([NIPS 2015 Tutorial slides][60], slide 63). You should thus always monitor error on a validation set during training and stop (with some patience) if your validation error does not improve enough.

## Gradient noise

Neelakantan et al. [][61][28] add noise that follows a Gaussian distribution $N(0, \sigma^2_t)$ to each gradient update:

$g_{t, i} = g_{t, i} + N(0, \sigma^2_t)$

They anneal the variance according to the following schedule:

$\sigma^2_t = \dfrac{\eta}{(1 + t)^\gamma}$

They show that adding this noise makes networks more robust to poor initialization and helps training particularly deep and complex networks. They suspect that the added noise gives the model more chances to escape and find new local minima, which are more frequent for deeper models.

# Conclusion

In this blog post, we have initially looked at the three variants of gradient descent, among which mini-batch gradient descent is the most popular. We have then investigated algorithms that are most commonly used for optimizing SGD: Momentum, Nesterov accelerated gradient, Adagrad, Adadelta, RMSprop, Adam, as well as different algorithms to optimize asynchronous SGD. Finally, we've considered other strategies to improve SGD such as shuffling and curriculum learning, batch normalization, and early stopping.

I hope that this blog post was able to provide you with some intuitions towards the motivation and the behaviour of the different optimization algorithms. Are there any obvious algorithms to improve SGD that I've missed? What tricks are you using yourself to facilitate training with SGD? **Let me know in the comments below.**

# Acknowledgements

Thanks to [Denny Britz][62] and [Cesar Salgado][63] for reading drafts of this post and providing suggestions.

# Printable version and citation

This blog post is also available as an [article on arXiv][3], in case you want to refer to it later.

In case you found it helpful, consider citing the corresponding arXiv article as: _Sebastian Ruder (2016). An overview of gradient descent optimisation algorithms. arXiv preprint arXiv:1609.04747._

# Translations

This blog post has been translated into the following languages:

Image credit for cover photo: [Karpathy's beautiful loss functions tumblr][64]

--------------------------------------------------------------------------------

1. H. Robinds and S. Monro, "A stochastic approximation method," Annals of Mathematical Statistics, vol. 22, pp. 400–407, 1951\. [↩︎][65]

2. Darken, C., Chang, J., & Moody, J. (1992). Learning rate schedules for faster stochastic gradient search. Neural Networks for Signal Processing II Proceedings of the 1992 IEEE Workshop, (September), 1–11\. [http://doi.org/10.1109/NNSP.1992.253713][66] [↩︎][67]

3. Dauphin, Y., Pascanu, R., Gulcehre, C., Cho, K., Ganguli, S., & Bengio, Y. (2014). Identifying and attacking the saddle point problem in high-dimensional non-convex optimization. arXiv, 1–14\. Retrieved from [http://arxiv.org/abs/1406.2572][68] [↩︎][69]

4. Sutton, R. S. (1986). Two problems with backpropagation and other steepest-descent learning procedures for networks. Proc. 8th Annual Conf. Cognitive Science Society. [↩︎][70]

5. Qian, N. (1999). On the momentum term in gradient descent learning algorithms. Neural Networks : The Official Journal of the International Neural Network Society, 12(1), 145–151\. [http://doi.org/10.1016/S0893-6080(98)00116-6][71] [↩︎][72]

6. Nesterov, Y. (1983). A method for unconstrained convex minimization problem with the rate of convergence o(1/k2). Doklady ANSSSR (translated as Soviet.Math.Docl.), vol. 269, pp. 543– 547\. [↩︎][73]

7. Bengio, Y., Boulanger-Lewandowski, N., & Pascanu, R. (2012). Advances in Optimizing Recurrent Networks. Retrieved from [http://arxiv.org/abs/1212.0901][74] [↩︎][75]

8. Sutskever, I. (2013). Training Recurrent neural Networks. PhD Thesis. [↩︎][76]

9. Duchi, J., Hazan, E., & Singer, Y. (2011). Adaptive Subgradient Methods for Online Learning and Stochastic Optimization. Journal of Machine Learning Research, 12, 2121–2159\. Retrieved from <http://jmlr.org/papers/v12/duchi11a.html> [↩︎][77]

10. Dean, J., Corrado, G. S., Monga, R., Chen, K., Devin, M., Le, Q. V, ... Ng, A. Y. (2012). Large Scale Distributed Deep Networks. NIPS 2012: Neural Information Processing Systems, 1–11\. [http://papers.nips.cc/paper/4687-large-scale-distributed-deep-networks.pdf][78] [↩︎][79] [↩︎][80]

11. Pennington, J., Socher, R., & Manning, C. D. (2014). Glove: Global Vectors for Word Representation. Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing, 1532–1543\. [http://doi.org/10.3115/v1/D14-1162][81] [↩︎][82]

12. Duchi et al. [3] give this matrix as an alternative to the _full_ matrix containing the outer products of all previous gradients, as the computation of the matrix square root is infeasible even for a moderate number of parameters (d). [↩︎][83]

13. Zeiler, M. D. (2012). ADADELTA: An Adaptive Learning Rate Method. Retrieved from [http://arxiv.org/abs/1212.5701][84] [↩︎][85]

14. Kingma, D. P., & Ba, J. L. (2015). Adam: a Method for Stochastic Optimization. International Conference on Learning Representations, 1–13\. [↩︎][86] [↩︎][87]

15. Heusel, M., Ramsauer, H., Unterthiner, T., Nessler, B., & Hochreiter, S. (2017). GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium. In Advances in Neural Information Processing Systems 30 (NIPS 2017). [↩︎][88]

16. Dozat, T. (2016). Incorporating Nesterov Momentum into Adam. ICLR Workshop, (1), 2013–2016\. [↩︎][89]

17. Huang, G., Liu, Z., Weinberger, K. Q., & van der Maaten, L. (2017). Densely Connected Convolutional Networks. In Proceedings of CVPR 2017\. [↩︎][90]

18. Johnson, M., Schuster, M., Le, Q. V, Krikun, M., Wu, Y., Chen, Z., ... Dean, J. (2016). Google's Multilingual Neural Machine Translation System: Enabling Zero-Shot Translation. arXiv Preprint arXiv:1611.0455\. [↩︎][91]

19. Reddi, Sashank J., Kale, Satyen, & Kumar, Sanjiv. On the Convergence of Adam and Beyond. Proceedings of ICLR 2018\. [↩︎][92]

20. Niu, F., Recht, B., Christopher, R., & Wright, S. J. (2011). Hogwild! : A Lock-Free Approach to Parallelizing Stochastic Gradient Descent, 1–22\. [↩︎][93]

21. Mcmahan, H. B., & Streeter, M. (2014). Delay-Tolerant Algorithms for Asynchronous Distributed Online Learning. Advances in Neural Information Processing Systems (Proceedings of NIPS), 1–9\. Retrieved from [http://papers.nips.cc/paper/5242-delay-tolerant-algorithms-for-asynchronous-distributed-online-learning.pdf][94] [↩︎][95]

22. Abadi, M., Agarwal, A., Barham, P., Brevdo, E., Chen, Z., Citro, C., ... Zheng, X. (2015). TensorFlow : Large-Scale Machine Learning on Heterogeneous Distributed Systems. [↩︎][96]

23. Zhang, S., Choromanska, A., & LeCun, Y. (2015). Deep learning with Elastic Averaging SGD. Neural Information Processing Systems Conference (NIPS 2015), 1–24\. Retrieved from [http://arxiv.org/abs/1412.6651][97] [↩︎][98]

24. LeCun, Y., Bottou, L., Orr, G. B., & Müller, K. R. (1998). Efficient BackProp. Neural Networks: Tricks of the Trade, 1524, 9–50\. [http://doi.org/10.1007/3-540-49430-8_2][99] [↩︎][100]

25. Bengio, Y., Louradour, J., Collobert, R., & Weston, J. (2009). Curriculum learning. Proceedings of the 26th Annual International Conference on Machine Learning, 41–48\. [http://doi.org/10.1145/1553374.1553380][101] [↩︎][102]

26. Zaremba, W., & Sutskever, I. (2014). Learning to Execute, 1–25\. Retrieved from [http://arxiv.org/abs/1410.4615][103] [↩︎][104]

27. Ioffe, S., & Szegedy, C. (2015). Batch Normalization : Accelerating Deep Network Training by Reducing Internal Covariate Shift. arXiv Preprint arXiv:1502.03167v3\. [↩︎][105]

28. Neelakantan, A., Vilnis, L., Le, Q. V., Sutskever, I., Kaiser, L., Kurach, K., & Martens, J. (2015). Adding Gradient Noise Improves Learning for Very Deep Networks, 1–11\. Retrieved from [http://arxiv.org/abs/1511.06807][106] [↩︎][107]

