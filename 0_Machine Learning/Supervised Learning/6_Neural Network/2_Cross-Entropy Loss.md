[rdipietro.github.io](https://rdipietro.github.io/friendly-intro-to-cross-entropy-loss/ "A Friendly Introduction to Cross-Entropy Loss")

# A Friendly Introduction to Cross-Entropy Loss

##  Predictive Power

After our discussion above, maybe we're happy with using cross entropy to measure the difference between two distributions $y$ and $\hat{y}$, and with using the total cross entropy over all training examples as our loss. In particular, if we let $n$ index training examples, the overall loss would be

$$ H({y^{(n)}}, {\hat{y}^{(n)}}) = \sum_n H(y^{(n)}, \hat{y}^{(n)}) $$

But let's look at another approach.

What if we want our objective function to be a direct measure of our model's predictive power, at least with respect to our training data?

One common approach is to tune our parameters so that the _likelihood_ of our data under the model is maximized. Since for classification we often use a [discriminative model][3], our "data" often just consists of the labels we're trying to predict. A model that often predicts the ground-truth labels given the inputs might be useful; a model that often fails to predict the ground-truth labels isn't useful.

Because we usually assume that our samples are [independent and identically distributed][4], the likelihood over all of our examples decomposes into a product over the likelihoods of individual examples:

$$ L({y^{(n)}}, {\hat{y}^{(n)}}) = \prod_n L(y^{(n)}, \hat{y}^{(n)}) $$

And what's the likelihood of the $n$-th example? It's just the particular entry of $\hat{y}^{(n)}$ that corresponds to the ground-truth label specified by $y^{(n)}$!

Going back to the original example, if the first training image is of a landscape, then $y^{(1)} = (1.0, 0.0, 0.0)^T$, which tells us that the likelihood $L(y^{(1)}, \hat{y}^{(1)})$ is just the first entry of $\hat{y}^{(1)} = (0.4, 0.1, 0.5)^T$, which is $\hat{y}^{(1)}_1 = 0.4$.

To keep going with this example, let's assume we have a total of four training images, with labels {landscape, something else, landscape, house}, giving us ground-truth distributions $y^{(1)} = (1.0, 0.0, 0.0)^T$, $y^{(2)} = (0.0, 0.0, 1.0)^T$, $y^{(3)} = (1.0, 0.0, 0.0)^T$, and $y^{(4)} = (0.0, 1.0, 0.0)^T$. Our model would predict four _other_ distributions $\hat{y}^{(1)}$, $\hat{y}^{(2)}$, $\hat{y}^{(3)}$, and $\hat{y}^{(4)}$, and the overall likelihood would just be

$$ L({y^{(1)}, y^{(2)}, y^{(3)}, y^{(4)}}, {\hat{y}^{(1)}, \hat{y}^{(2)}, \hat{y}^{(3)}, \hat{y}^{(4)}}) = \hat{y}^{(1)}_1 , \hat{y}^{(2)}_3 , \hat{y}^{(3)}_1 , \hat{y}^{(4)}_2 $$

Maybe the last section made us happy with minimizing cross entropy during training, but are we still happy?

Why shouldn't we instead maximize the likelihood $L({y^{(n)}}, {\hat{y}^{(n)}})$ of our data?

[1]: https://rdipietro.github.io#Predictive-Power
[2]: https://rdipietro.github.io/images/predict.jpg
[3]: https://en.wikipedia.org/wiki/Discriminative_model
[4]: http://math.stackexchange.com/questions/466927/independent-identically-distributed-iid-random-variables

