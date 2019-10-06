# Dual SVM

[机器学习技法笔记(3)-对偶SVM \| 天空的城](https://shomy.top/2017/02/17/svm-02-dual/)
[支持向量机：Duality « Free Mind](http://blog.pluskid.org/?p=702)

对偶SVM主要解决在非线性转换之后维度变高二次规划问题求解困难的问题。

在非线性分类问题中，我们一般作非线性转换zn=ϕ(xn)，从而在z空间里面进行线性分类，在非线性转换过程一般是一个升维的过程，将z的维度一般设为d̃ , 一般情况 d̃ >>d，(比如2维变5维，3维变19维等是一个非线性升高)，这样在高维空间内部，在二次规划问题中的就会有d̃ +1个变量，那么权值维度同样也为d̃ +1维，以及N个约束，此外Q 矩阵维度会十分大，达到d̃ 2，因此，一旦d̃变大了， 二次规划问题求解就会变得很困难。

通过引入对偶问题以及kernel，消除SVM解决非线性问题转换到高维空间中对d̃依赖问题。

## 对偶的引入

我们的目标是将原始的非线性问题，转化为另外一个对等问题，使得新的对等问题的规模或者说复杂度(主要指二次规划问题的规模，比如变量数目，__约束条件数目等)仅仅与原始数据的规模(样本量)有关，与转换的高维空间无关__，如下:
![svm02-1.png](http://cdn.htliu.cn/svm02-1.png)

## 拉格朗日函数
在引入对偶问题之前，在回顾上一节的二次规划问题SVM:
![1351141994_1802.jpg](https://wizardforcel.gitbooks.io/dm-algo-top10/content/img/1351141994_1802.jpg)

我们从另一个角度考虑，这里我们引入拉格朗日函数(Lagrange Function)来将约束条件和目标函数结合到一个式子:

$\mathcal{L}(b, \textbf{w}, \mathbf{\alpha}) = \frac{1}{2}\textbf{w}^T \textbf{w} + \sum\limits_{n=1}^{N}\alpha_n(1-y_n(\mathbf{w}^T\mathbf{x}+b)), \quad \alpha_n \geq 0$

其中α为拉格朗日乘子(Lagrange Multiplier)。<有几个constraint condition就有几个拉格朗日乘子>

原始SVM二次规划问题与下式等价，即:

$\mathbf{SVM} \equiv \min \limits_{b, \mathbf{w}} (\max \limits_{\alpha_n \geq 0} \mathcal{L}(b, \mathbf{w},\alpha))$

首先右式表示多元变量的优化问题，首先在固定b,w下，通过调整α,使得L(b,w,α)最大，进而再优化外面的min部分。

再看max部分，分两种情况叙述:

1. w,b不满足约束条件$y_n(\mathbf{w}^Tx+b) \geq 1$，即: 1−yn(wTx+b)>0，再结合αn≥0,很显然, $\max(\mathcal{L}) \to \infty$
2. w,b满足$y_n(\mathbf{w}^Tx+b) \leq 1$，那么在求max的时候，显然需要$\alpha_n(1-y_n(\mathbf{\mathbf{w}^T\mathbf{x}+b})) = 0$,此时$\max(\mathcal{L}) = \frac{1}{2}\textbf{w}^T \textbf{w}$
   显然，求完了max之后，在优化外层求min的时候，对于上述第一种∞情况，肯定不会是最优解，因此最后的优化方程为: $\frac{1}{2}\textbf{w}^T \textbf{w}$，因此与原始问题等价。

这样，我们就把原始的含有约束条件的优化问题，转为不含条件的表达式。



## 对偶问题转化
接着化简，因为max≥any，则对于任意一个给定的α′，下式显然成立:

$$\min \limits_{b, \mathbf{w}} (\max \limits_{\alpha_n \geq 0} \mathcal{L}(b, \mathbf{w},\alpha)) \geq \min\limits_{b, \mathbf{w}} \mathcal{L}(b, \mathbf{w},\alpha^{'})$$



既然上述的α′是任意给定的，那么左式也一定大于等于其中的那个最大的，so we add a max in front of the right hand side pard:
$$\min \limits_{b, \mathbf{w}} (\max \limits_{\alpha_n \geq 0} \mathcal{L}(b, \mathbf{w},\alpha)) \geq \max\limits _{all \ \alpha_n \geq 0} (\min\limits_{b, \mathbf{w}} \mathcal{L}(b, \mathbf{w},\alpha^{'}))$$



到此为止: 观察上述不等式，最大化与最小化做了交换，这样的不等式，__右式就称为左式的对偶问题，左式的下限就是右式__

此外，**上述不等式关系属于一种弱对偶关系(Weak Duality)** 
如果满足下面的几个条件，则可以去掉不等号，从而转为强对偶关系(Strong Duality)：

- 原始问题为是凸优化问题 (convex primal)
- 原始问题线性可分的(feasible primal)
- 约束条件是线性的(Linear constraints)
显然，原始问题满足以上强对偶条件，也就是说存在一组最优解w,b,α同时满足式子两边。到此，我们将原始的问题转化为其如下的对偶问题:

$$\max\limits_{all \ \alpha_n \geq 0}{\left ( \min\limits_{b,\mathbf{w}}{ \underbrace{\frac{1}{2}\mathbf{w}^T\mathbf{w}+\sum\limits_{n=1}^{N}\alpha_n(1-y_n(\mathbf{w}^T\mathbf{z}_n+b))}_{\mathcal{L}(b, \mathcal{w}, \alpha)}}\right)}$$



- 为什么要将原始问题，转换为对偶问题去求解呢？

开始说了,目标是降低算法的规模或者说是复杂度。原始问题的最关于后需要解决的$\min\limits_{b,\mathbf{w}}$部分的复杂度是with respect to w的，也就是高维空间的维度，求解很困难。而对偶问题最后要解决的$\max\limits_{\alpha}$则是关于αn的，复杂度跟**样本数量N**有关，在非线性分类中一般情况升维会达到很大的数量级(比如后续的高斯核是无穷维)，远远大于样本数量，这个时候用对偶问题来求解就会容易。

此外，如果是线性分类且样本维度低于样本数量的话，那就直接使用原始问题求解即可，无需对偶。



## 求解对偶问题
### 最小化
固定α, 对拉格朗日函数L最小化(凸函数)，L含有b,w两个变量，因此分别求一阶微分:

首先对b求偏微分，得到:

$$
\frac{\partial\mathcal{L}(b, \mathbf{w}, \alpha)}{\partial \ b} = \sum\limits_{n=1}^{N}\alpha_ny_n = 0
$$



因此我们最终得到的最优解应该满足上述条件，接着作一步转化:

$$\min\mathcal{L}(b, \mathbf{w},\alpha) = \min \frac{1}{2}\mathbf{w}^T\mathbf{w} + \sum\alpha_n(1-y_n\mathbf{w}^T\mathbf{z}^n) -  {b*\sum\alpha_ny_n}$$



既然我们的最优解需要满足∑αnyn=0，那么上式也应该满足，那么蓝色部分就会变成0，问题得到了简化。接着对w求微分如下:

$$
\frac{\partial\mathcal{L}(b, \mathbf{w}, \alpha)}{\partial \ \mathbf{w}} = \mathbf{w} - \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n = \mathbf{0}
$$

即最优解满足: $\mathbf{w} = \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n$，接着继续化简minL:

$$
\min\mathcal{L}(b, \mathbf{w},\alpha) $$
$$
$$\min \frac{1}{2}\mathbf{w}^T\mathbf{w} + \sum\alpha_n(1-y_n\mathbf{w}^T\mathbf{z}^n)$$

$$
\min \frac{1}{2}\mathbf{w}^T\mathbf{w} - \sum \alpha_ny_n\mathbf{z}^n\mathbf{w}^T+ \sum\alpha_n
$$

$$\Leftrightarrow {\min} -\frac{1}{2}\mathbf{w}^T\mathbf{w}  + \sum\limits_{n=1}^{N}\alpha_n$$

$$
-\frac{1}{2} \Arrowvert \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n\Arrowvert + \sum\limits_{n=1}^{N}\alpha_{n}
$$



经过分别求偏微分，并且将得到的条件代入到优化方程，最终去掉了min，仅仅与α有关。
现在我们的问题化简至下式:


$$\begin{align*}\max\limits_{\alpha_n \geq 0, \  \sum\alpha_ny_n =0, \ \mathbf{w}=\sum\alpha_ny_n\mathbf{z}^n}\left( -\frac{1}{2}\Arrowvert\sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n\Arrowvert^2+ \sum\limits_{n=1}^{N}\alpha_{n}\right)\end{align*}$$



至此最小化部分完成，完全转为了α的问题。






### 最大化
上一步的最小化结束了，这一部分做外层的最大化，这里仅仅与α有关。首先做一个转化，变为求min，然后展开平方如下:

$$\begin{align} \min\limits_{\alpha} & \quad \frac{1}{2}\sum\limits_{n=1}^{N}\sum\limits_{m=1}^{N}{\alpha_n\alpha_m} y_ny_m\mathbf{z}_n\mathbf{z}_m {-}\sum\limits_{n=1}^{N}\alpha_n \\ s.t. & \quad \alpha _n \geq 0, \ \sum\limits_{n=1}^{N}\alpha_ny_n  =0 \end{align}$$

相当与将条件再提出来，就转化为QP问题求解！并且上式中N个变量α1...αN，以及N+1个约束条件，这样就看着摆脱了高维，求解相对容易，这便得到就是标准SVM对偶问题。

下面便是根据QP问题的格式，写出对应的Q,p,A,c：

<img src="http://cdn.htliu.cn/svm02-3.png" alt="svm02-3.png" style="zoom: 67%;" />

这里需要说明一下:

- qm,n表示矩阵Q的第m行第n列的元素
- 对于$\sum y_n\alpha_n =0$，改为两个不等式条件: $\sum y_n\alpha_n$ <=0$, $\sum y_n\alpha_n$ >=0$, 因此一共有N+2个约束条件。





### 模型求解
利用QP Solver 就可以解决上述的二次规划问题，得到最优的 $\alpha=[\alpha_1...\alpha_N]$，下面介绍如何根据α求解最优的w,b，进而得到模型。

到这里整理一下已经有的条件:

1) 对于原始问题有: $y_n(\mathbf{w}^T\mathbf{z}_n+b) \geq 1$
2) 对于对偶问题有: αn≥0
3) 对于对偶问题内层的min优化求解中，有:

$\sum y_n\alpha_n =0$; $\sum \alpha_ny_n =0; \ \mathbf{w}=\sum\alpha_ny_n\mathbf{z}_n$

4) 对于原始问题内层的max问题中：上面得到结论有:$y_n(\mathbf{w}^T\mathbf{z}_n+b) =0$，被称为complementary slackness

注:上述条件即使二次规划问题中的**KKT条件(Karush-Kuhn-Tucker Conditions)**

根据条件3，便可得到 $\mathbf{w} = \sum\limits_{n=1}^{N}\alpha_ny_n\mathbf{z}_n$

根据条件4，我们知道αn以及$1- y_n(\mathbf{w}^T\mathbf{z}_n+b)$ 至少有一个为0，我们只需要找到一个αn>0的数据点，这样就会有:1−yn(wTzn+b)=0，进而得到:

$b = y_n - \mathbf{w}^T \mathbf{z}_n, \quad \alpha_n > 0$

这里需要注意的是: αn>0的点满足$y_n(\mathbf{w}^T\mathbf{z}_n + b) = 1$, 即SV(Support Vector)，这也就说明了我们只需要SV就可以得到w,b，这时候其它的点不需要。

简略总结一下标准对偶SVM的流程

1. 根据训练数据，(非线性转换$z_n = \phi(x_n)$，写出QP问题的Q,p,a,c
2. 使用QP Solover 求出最优的α
3. 根据条件约束，求出w,b
4. 得到模型：$g_{svm} = sign(\mathbf{w}^T\phi(\mathbf{x})+b)$





### 数据表示模型
在开始介绍SVM的时候，就是以PLA为基础的，下面看看二者:
<img src="http://cdn.htliu.cn/svm02-4.png" alt="svm02-4.png" style="zoom:50%;" />

首先在Dual SVM中，w是以$y_n\mathbf{z}_n$的线性组合求解的，其中参数为αn;_ 在PLA中 同样，每个分类错误的点，都会加上$y_n\mathbf{z}_n$，_ 因此实质上w同样线性组合求解得到。

其实还有很多线性模型或者算法，比如前面的Linear Regresson, Logistic Regression等如果初始化的w=0，最终得到的最优解同样是数据的线性组合。

这个时候，我们就称参数w是被训练数据表示出来的(represented by data)，只不过__SVM的权重参数只需通过αn>0的SV__表示即可。(PS：数据表示的模型可能造成数据泄漏?)



## 后续
这一节主要讲的是标准对偶SVM，旨在解决高维空间下，二次规划问题求解困难的问题，下面对比Linear SVM与Dual SVM:
<img src="http://cdn.htliu.cn/svm02-5.png" alt="svm02-5.png" style="zoom:67%;" />

看起来我们的对偶问题的规模成功做到了只与数据样本量N有关，与高维空间的维度d̃无关，其实不然，实际内部的计算量还是很大的:

__Q矩阵的元素: $q_{n,m} = y_ny_m\mathbf{z}_m\mathbf{z}_n$，实际为$R^{\tilde{d}}$内的内积!__ 还是没有摆脱高维。不过对偶问题相比原始问题在非线性数据的处理上，求解已经更加容易了。 至于如何在求解$q_{n,m}$的时候，彻底不再依赖d̃ ，那就是下节的内容了，引入带核的SVM。









