# Kernel

Kernel = transform + inner product

Kernel Trick: You want to work with degree 2 polynomial features,Á(x). Then, your dot product will be operate using vectors in a space of dimensionality __n(n+1)/2__. (take all point to a second degree term) 

The kernel trick allows to save time/space and compute dot products in an N dimensional space. Not just for 2 polynomial

核函数![equation?tex=K%28v_1%2Cv_2%29+%3D+%3Cv_1%2Cv_2%3E%5E2](https://www.zhihu.com/equation?tex=K%28v_1%2Cv_2%29+%3D+%3Cv_1%2Cv_2%3E%5E2)，即“内积平方”。
这里面![equation?tex=v_1%3D%28x_1%2Cy_1%29%2C+v_2%3D%28x_2%2Cy_2%29](https://www.zhihu.com/equation?tex=v_1%3D%28x_1%2Cy_1%29%2C+v_2%3D%28x_2%2Cy_2%29)是二维空间中的两个点。

这个核函数对应着一个二维空间到三维空间的映射，它的表达式是：
![equation?tex=P%28x%2Cy%29%3D%28x%5E2%2C%5Csqrt%7B2%7Dxy%2Cy%5E2%29](https://www.zhihu.com/equation?tex=P%28x%2Cy%29%3D%28x%5E2%2C%5Csqrt%7B2%7Dxy%2Cy%5E2%29)

可以验证，

![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%7D+%3CP%28v_1%29%2CP%28v_2%29%3E+%26%3D+%5C%2C+%3C%28x_1%5E2%2C%5Csqrt%7B2%7Dx_1y_1%2Cy_1%5E2%29%2C%28x_2%5E2%2C%5Csqrt%7B2%7Dx_2y_2%2Cy_2%5E2%29%3E+%5C%5C+%26%3D+%5C%2C+x_1%5E2x_2%5E2+%2B+2x_1x_2y_1y_2%2By_1%5E2y_2%5E2+%5C%5C+%26%3D+%5C%2C+%28x_1x_2+%2B+y_1y_2%29%5E2+%5C%5C+%26%3D+%5C%2C+%5C%2C+%3Cv_1%2Cv_2%3E%5E2+%5C%5C+%26%3D+%5C%2C+K%28v_1%2Cv_2%29+%5Cend%7Balign%7D)



核技巧是一个非常纯粹的数学方法，不应该一上来就扯上SVM。因为这个技巧不仅应用在SVM中，还有其他领域也有应用。它解决了数据映射到高维空间之后点积的计算量过于复杂的问题。很多人啰啰嗦嗦说了一大堆，把原本很简单的东西搞的很复杂，其实这玩意很好理解。

给定两个向量xi和xj，我们的目标是要计算他们的内积$I = <x_i, x_j>$。现在假设我们通过某种非线性变换：$\Phi : x \rightarrow \phi(x)$

把他们映射到某一个高维空间中去，那么映射后的向量就变成：ϕ(xi)和ϕ(xj), 映射后的内积就变成：$I’ = <\phi(x_j),\phi(x_j)>$。现在改如何计算映射后的内积呢？

传统方法是先计算映射后的向量ϕ(xi)和ϕ(xj)，然后再计算它俩的内积。但是这样做计算很复杂，因为映射到高维空间后的数据维度很高。比如，假设xi和xj在映射之后都是一个1×10000维的向量，那么他们的内积计算就需要做10000次加法操作和10000次乘法操作，显然复杂度很高。于是，数学家们就想出一个办法：能不能在原始空间找到一个函数$K(x_i,x_j)$使得$K(x_i,x_j)$=<ϕ(xi),ϕ(xj)>呢？ 如果这个函数存在，那么我们只需要在低维空间里计算函数K(x_i,x_j)的值即可，而不需要先把数据映射到高维空间，再通过复杂的计算求解映射后的内积了。

庆幸的是，这样的函数是存在的。这样一来计算的复杂度就大大降低了，这种简化计算的方法被称为核技巧（The Kernel Trick），而函数K就是核函数（Kernel Function）

其中ϕ(x)是对x做变换的函数，有些变换会将样本映射到更高维的空间，如果这个高维空间内x1与x2是线性可分的，那么我们就做了一次成功的变换。核函数是二元函数，输入是变换之前的两个向量，其输出与两个向量变换之后的内积相等（这个性质非常重要）。



## 与SVM的关系
$$\sum_{i=1}^{N}\sum_{j=1}^{N}\alpha_i\alpha_jy_iy_j\phi(x_i)^{T}phi(x_j)-\sum_{i=1}^{N}\alpha_i\\\text{s.t.}\\0\leq\alpha_1\leq C\\ \sum_{i=1}^{N} y_{i}\alpha _{i}=0$$

观察上面的公式你会发现里面有一个内积运算$\phi  \left ( x_{i} \right )^{T}\phi \left ( x_{j} \right )$



核函数的满足条件
Mercer 定理：任何半正定的函数都可以作为核函数。所谓半正定的函数$f(x_i,x_j)$，是指拥有训练数据集合$x_1,x_2,…x_n$，我们定义一个矩阵的元素$a_{ij} = f(x_i,x_j)$，这个矩阵是n×n的，如果这个矩阵是半正定的，那么f(xi,xj)就称为半正定的函数。

这个mercer定理不是核函数必要条件，只是一个充分条件，即还有不满足mercer定理的函数也可以是核函数。



https://shomy.top/2017/02/19/svm03-kernel-trick/

## 核的引入

由上一节可以知道，计算量集中在Q矩阵中元素的计算:

$\begin{align}q_{n,m} &= y_ny_m\mathbf{z}_n^T\mathbf{z}_m \\ &=y_ny_m\phi(\mathbf{x}_n)^T\phi(\mathbf{x}_m)\end{align}$

这里分为两步，首先是空间转换，然后在高维空间中进行内积运算，这个计算量会很大的，尤其是后面还会有无穷维的转换，那么这一步根本无法完成。

如果我们将这两步合成一步的话， 就会降低计算量。下面以二阶多项式转换为例介绍: 

假设原始数据有d维: x=(x1...xd),

$\phi_2(\mathbf{x}) =(1, x_1,x_2...,x_d, x_1^2,x_1x_2,...,x_1x_d,...,x_2x_1,x_2^2,...,x_d^2)$

那么对两个数据x,x′计算内积如下:

$$\phi_2(\mathbf{x})\phi_2(\mathbf{x}^{'}) &= 1 + \sum\limits_{i=1}^{d}x_ix_{i}^{'} + \sum\limits_{i=1}^{d}\sum\limits_{j=1}^{d}x_ix_jx_i^{'}x_j^{'} \\ &= 1 + \sum\limits_{i=1}^{d}x_ix_{i}^{'} + \sum\limits_{i=1}^{d}x_ix_i^{'}\sum\limits_{j=1}^{d}x_jx_j^{'} \\ &=1 + \mathbf{x}\mathbf{x}^{'} + (\mathbf{x}\mathbf{x}^{'})^2$$

可以发现，我们将转换与计算内积的过程转为了一步完成，就是上式，而且计算仅仅在原始维度上进行，复杂度从O(d~)降到了O(d)，完全去除了对d~的依赖。

我们称Kϕ(x,x′)=ϕ2(x)ϕ2(x′)为核函数(Kernel Function)。比如Kϕ2=1+xx′+(xx′)2 。**核函数主要作用就会使得内积计算都在原始维度上，与高维空间无关，属于一种内积的运算简化技巧(Kernel Trick)。**

有了核函数，我们便可以将Q矩阵改写如下:

$q_{n,m} = y_ny_mK(x_n,x_m)$

其余变量不变，求解新的二次规划问题，得到拉格朗日乘子α，由于我们跳过了转换过程，所以无法计算单独的z，因此利用α求w,b的方法也得做一定修改。

先看b,利用某个SV即$\alpha_s >0$来求:

$\begin{align}b &= y_s - \mathbf{w}^T\mathbf{z}_s \\&=y_s - \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n\mathbf{z}_s \\&=y_s - \sum\limits_{n=1}^{N}\alpha_ny_nK(\mathbf{x}_n, \mathbf{x}_s) \end{align}$

这样就得到了b的表达式。

再看w=∑αnynzn,貌似无法求解，因为这里面的zn没有办法求解。但是从另一个角度想，我们计算w的目的就是为了计算wTz，那么我们整体计算看看得到什么:

$\mathbf{w}^T\mathbf{z} = \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n\mathbf{z} = \sum\limits_{n=1}^{N}\alpha_ny_nK(\mathbf{x}_n\mathbf{x})$

这样也间接完成了我们的目的。这样利用Kernel Function，我们就避开了在高维空间的计算，甚至不需要知道用的什么映射，就直接在原始低维数据进行等价计算。

总结一下核机制SVM的一般流程：

![img](http://cdn.htliu.cn/svm03-1.png)

再来看看每一步的复杂度，如下，第一步的O(N2)是因为Q矩阵的规模是N∗N的。

![img](http://cdn.htliu.cn/svm03-2.png)

上面举的例子是二阶的核函数，下面介绍几个常用的核函数。



## 多项式核

我们将Kϕ2=1+xx′+(xx′)2 加一些系数，得到更加简单更加常用的K2(x,x′)=(1+γxx′)2，如下:

<img src="http://cdn.htliu.cn/svm03-3.png" alt="img" style="zoom:67%;" />

需要注意的是，K2,Kϕ2这两种核函数的能力本质上是相同的，也就是说解决同一类问题结果类似，只不过最后求出的边界不同。

此外，多项式核可以推向更一般的情况，如下:

<img src="http://cdn.htliu.cn/svm03-4.png" alt="img" style="zoom:67%;" />

其中ζ,γ是超参数，Q是阶数。

这里面有一个特殊的: 线性核(Linear Kernel)，计算简单高效:

K1(x,x′)=(0+1∗xx′)1=xx′,ζ=0,γ=1,Q=1

在实际应用中，还是需要遵循**Linear First**的原则，首先尝试**Linear Kernel**，高阶的核函数虽然能力强，但是相对容易造成OverfFitting。



## 高斯核

除了多项式核函数，另外常用的核函数还有高斯核函数，也叫做径向基函数(Radial Basis Function, RBF):

$K(\mathbf{x},\mathbf{x}^{'}) = exp(-\gamma\Arrowvert \mathbf{x}-\mathbf{x}^{'}\Arrowvert^2), \quad \gamma >0$

RBF核实现的是无限维的转换，证明思路就是将高斯核函数可以表示为无穷项的和，并且需要是两个向量内积的形式。下面以一维为例，即x=(x1)，基于泰勒展开来证明：

![img](http://cdn.htliu.cn/svm03-5.png)

高斯核$K(\mathbf{x},\mathbf{x}^{'}) = exp(-\gamma\Arrowvert \mathbf{x}-\mathbf{x}^{'}\Arrowvert^2), \quad \gamma >0$ 只有一个参数γ，在实际应用中，γ一般不应该太大，否则很容易过拟合，还有一点高斯核完成的是到无穷维的转换，因此它的描述能力很强，可以作出很复杂的边界，但是由于是无穷维的转换，这个转换过程类似黑盒子，也就是说w也是无穷维的，我们无法计算出它，因此也就缺乏了物理意义。



## 核比较

下面对线性核，多项式核，高斯核(RBF)做个对比。

**线性核(Linear Kernel)** K(x,x′)=xx′:

![img](http://cdn.htliu.cn/svm03-6.png)

- 优点: 泛化性能好，计算快速，应该优先考虑，解释性强，因为各个特征的重要性就直接体现在权重内。
- 缺点: 模型太简单，限制较大，比如线性不可分的数据

主要用于线性可分的情形，速度快，优先考虑。



**多项式核(Ploy Kernel)**

<img src="http://cdn.htliu.cn/svm03-7.png" alt="img" style="zoom:50%;" />

- 优点: 比线性核更加Powerful， 可以解决线性不可分的数据。
- 缺点: 超参数多(ζ,γ,Q)，不易选择; 另外 Q如果比较大的时候，矩阵的数值范围过大或者过小，不易计算，此外需要注意过拟合

一般使用的比较小的Q。 不过使用较少，因为速度不如Linear，能力不如RBF



**高斯核(RBF Kernel)**

<img src="http://cdn.htliu.cn/svm03-8.png" alt="img" style="zoom:50%;" />

- 优点: 分类更加Powful，可以得到很复杂的边界，; 只有一参数γ，模型选择更加简单。
- 缺点: 解释性差，因为无法计算无限维的w,无法知道各个特征的权重; 过拟合更加严重，需要严格控制γ。

与Linear Kernel 一样，很常用，用于线性不可分的情形，参数多，因为计算量多，相对耗时较多。

将原始空间映射为无穷维空间。不过，如果 σ 选得很大的话，高次特征上的权重实际上衰减得非常快，所以实际上（数值上近似一下）相当于一个低维的子空间；反过来，如果 σ 选得很小，则可以将任意的数据映射为线性可分——当然，这并不一定是好事，因为随之而来的可能是非常严重的过拟合问题。不过，总的来说，通过调控参数 σ ，高斯核实际上具有相当高的灵活性，也是使用最广泛的核函数之一。



## 总结

一般情况，使用线性核或者高斯核，RBF更加通用，参数好的话，基本会比Linear好，但是耗时多。

吴恩达NG教程中提到的以下经验:

- 如果Features数量大，与样本数量持平或者大于，这时候一般用Linear Kernal
- 如果Features较少，采用RBF Kernel
- 如果样本量过大的话，使用带Kernel的SVM优势就不大了。

最好的方式是根据具体数据来尝试哪种效果最好。

那么除了上述的Linear Kernel，Poly Kernel，RBF Kernel 之外，还有其它的核函数吗？

我们已经知道核函数是为了简化内积运算的trick，只知道K(x,x′)=ϕ(x)ϕ(x′)，此外我们知道向量内积就是描述向量的相似度，(比如向量垂直,内积为0; 相反，内积为负数)，那么我们能否从某个相似性出发，随意指定某个函数来作为相似度的描述？

Not Really。**Mercer Condition**是用来判断一个函数能否作为核函数的条件。我们将数据写成核矩阵K的形式:Ki,j=K(xi,xj)，整个矩阵如下:

<img src="http://cdn.htliu.cn/svm03-9.png" alt="img" style="zoom:67%;" />

很显然，K应该满足**对称性，半正定**的性质，事实上，**对称，半正定**也是K可以作为核矩阵的充要条件(证明略过)，虽然可以指定某个函数作为核函数，但是在实际中，Linear Kernel和RBF Kernel已经足够用了。

## 后续

到这里似乎SVM的分类问题貌似解决了，但是正如前面提到的，一直存在一个OverFitting的问题，比如说如果存在某些错误噪音数据，那么就算转到高维空间仍然存在线性不可分的情况，此时，就会产生很复杂的分类边界，造成OverFitting，但是我们更希望，忽略个别的错误数据，如下图:

<img src="http://cdn.htliu.cn/svm04-1.png" alt="img" style="zoom:50%;" />