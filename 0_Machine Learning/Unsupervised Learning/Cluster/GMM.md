# Gaussian Mixture Model GMM

k-means的结果是每个数据点被 assign 到其中某一个 cluster 了，而GMM则给出这些数据点被 assign 到每个 cluster 的概率，又称作 soft assignment

‌作为一个流行的算法，GMM有它自己的一个相当体面的归纳偏执(Bias)了。其实它的假设非常简单，顾名思义，Gaussian Mixture Model ，就是假设数据服从 Mixture Gaussian Distribution ，换句话说，数据可以看作是从数个Gaussian Distribution中生成出来的。实际上，我们在 [K-means](http://blog.pluskid.org/?p=17)和 [K-medoids](http://blog.pluskid.org/?p=40)两篇文章中用到的那个例子就是由三个 Gaussian 分布从随机选取出来的。实际上，从[中心极限定理](http://en.wikipedia.org/wiki/Central_limit_theorem)可以看出，Gaussian 分布（正态分布）这个假设其实是比较合理的，除此之外，Gaussian 分布在计算上也有一些很好的性质，所以，虽然我们可以用不同的分布来随意地构造 XX Mixture Model ，但是还是 GMM 最为流行。另外，Mixture Model 本身其实也是可以变得任意复杂的，通过增加 Model 的个数，我们可以任意地逼近任何连续的概率密分布。

每个 GMM由 K个 Gaussian 分布组成，每个 Gaussian 称为一个“Component”，这些 Component 线性加成在一起就组成了 GMM 的概率密度函数：

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lpo8m4yvEtRrSGkyAPg%2F-LqPrDJew-qGXQRNek1R%2F-LqQ6iR9WrqB-T9DHpq8%2Fimage.png?alt=media&token=9ce62eb2-74b4-451e-b94a-008e4994a577)

根据上面的式子，如果我们要从 GMM 的分布中随机地取一个点的话，实际上可以分为两步：

首先随机地在这 K个 Component 之中选一个，每个 Component被选中的概率实际上就是它的系数$\pi_k$, 选中了 Component 之后，再单独地考虑从这个 Component 的分布中选取一个点就可以了─这里已经回到了普通的 Gaussian 分布，转化为了已知的问题。

那么如何用GMM来做clustering呢？

**假定data是由 GMM 生成出来的，那么我们只要根据数据推出 GMM 的概率分布来就可以了，然后 GMM 的K个 Component 实际上就对应了K个 cluster**了。根据数据来推算概率密度通常被称作 density estimation ，特别地，当我们在已知（或假定）了概率密度函数的形式，而要估计其中的参数的过程被称作“参数估计”。

现在假设 N个数据点，并假设它们服从某个分布 P(x)，现在要确定里面的一些参数的值，例如在GMM中，我们就需要确定$\pi_k$, $\mu_k$, $\Sigma_k$这些参数。 

我们的想法是，找到这样一组参数，它所确定的概率分布生成这些给定的数据点的概率最大，而这个概率实际上就等于$\prod_{i=1}^N p(x_i)$ 我们把这个乘积称作[似然函数 (Likelihood Function)](http://en.wikipedia.org/wiki/Likelihood_function)。通常单个点的概率都很小，许多很小的数字相乘起来在计算机里很容易造成浮点数下溢，因此通常会对其取对数，把乘积变成加和$\sum_{i=1}^N \log p(x_i)$，得到 log-likelihood function
接下来我们只要将这个函数最大化（通常的做法是求导并令导数等于零，然后解方程），亦即找到这样一组参数值，它让似然函数取得最大值，我们就认为这是最合适的参数，这样就完成了参数估计的过程。

下面让我们来看一看 GMM 的 log-likelihood function 

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lpo8m4yvEtRrSGkyAPg%2F-LqPrDJew-qGXQRNek1R%2F-LqQ7RhVRweejeBMu8mX%2Fimage.png?alt=media&token=240ee707-4f04-48fa-9d93-d84018bb58ad)
‌

由于在对数函数里面又有加和，我们没法直接用求导解方程的办法直接求得最大值。为了解决这个问题，我们采取之前从 GMM 中随机选点的办法：分成两步，实际上也就类似于 [K-means](http://blog.pluskid.org/?p=17)的两步。

1. 估计数据由每个Component生成的概率（并不是每个Component 被选中的概率）：对于每个数据$x_i$，它由第k个 Component 生成的概率为
![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lpo8m4yvEtRrSGkyAPg%2F-LqPrDJew-qGXQRNek1R%2F-LqQ7bJzeu2l5VpZ7hod%2Fimage.png?alt=media&token=1ae3bf24-6825-4eb0-8811-530bdc2aea2c)

由于式子里的 $\mu_k$, $\Sigma_k$也是需要我们估计的值，我们采用迭代法，在计算$\gamma(i, k)$ 的时候我们假定 $\mu_k$, $\Sigma_k$ 均已知，我们将取上一次迭代所得的值（或者初始值）。

2. 估计每个 Component 的参数：现在我们假设上一步中得到的 $\gamma(i, k)$ 就是正确的“数据$x_i$由 Component  k生成的概率”，亦可以当做该 Component 在生成这个数据上所做的贡献，或者说，我们可以看作 $x_i$这个值其中有 $\gamma(i, k)x_i$这部分是由 Component k所生成的。集中考虑所有的数据点，现在实际上可以看作 Component 生成了 $\gamma(1, k)x_1, \ldots, \gamma(N, k)x_N$这些点。由于每个 Component 都是一个标准的 Gaussian 分布，可以很容易分布求出最大似然所对应的参数值：
![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-Lpo8m4yvEtRrSGkyAPg%2F-LqPrDJew-qGXQRNek1R%2F-LqQ82V48K3kdX0XL0kM%2Fimage.png?alt=media&token=8b6201d5-8122-4023-ad03-c454bc3eac7c)

其中 $N_k = \sum_{i=1}^N \gamma(i, k)$，并且 $\pi_k$也顺理成章地可以估计为 $N_k/N$

3. 重复迭代前面两步，直到似然函数的值收敛为止。


