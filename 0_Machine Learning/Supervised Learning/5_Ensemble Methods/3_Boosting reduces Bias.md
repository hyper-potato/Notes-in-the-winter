# Boosting

## Boosting reduces Bias

http://www.52caml.com/head_first_ml/ml-chapter6-boosting-family/

<img src='https://miro.medium.com/max/2552/1*8T4HEjzHto_V8PrEFLkd9A.png'>

Bagging is a simple ensembling technique in which we build many independent predictors/models/learners and combine them using some model averaging techniques. (e.g. weighted average, majority vote or normal average)

Boosting is an ensemble technique in which the predictors are not made independently, but **sequentially**.

Boosting方法就是从弱学习算法出发，反复学习，得到一系列弱分类器，然后组合弱分类器，得到一个强分类器。Boosting方法在学习过程中通过改变训练数据的权值分布，针对不同的数据分布调用弱学习算法得到一系列弱分类器。

这里面有两个问题需要回答：

1. 在每一轮学习之前，如何改变训练数据的权值分布？

  Ω 针对第一个问题，Adaboost算法的做法是：

   **提高那些被前一轮弱分类器错误分类样本的权值，而降低那些被正确分类样本的权值。**如此，那些没有得到正确分类的样本，由于其权值加大而受到后一轮的弱分类器的更大关注。

   **trying to avoid making the same mistake in the next round(penalty)**

   

2. 如何将一组弱分类器组合成一个强分类器？

   第二个问题，弱分类器的组合，AdaBoost采取**加权多数表决**的方法。具体地：

   **加大分类误差率小的弱分类器的权值，使其在表决中起较大的作用；减小分类误差率大的弱分类器的权值，使其在表决中起较小的作用。**



### **Boosting四大家族**

Boosting是一类方法。这里按照__损失函数__的不同，将其细分为若干类算法

|       Name       |              LOSS FUCTION              |                    导数(Derivative)                    |                 目标函数f∗                 |       算法        |
| :--------------: | :------------------------------------: | :----------------------------------------------------: | :----------------------------------------: | :---------------: |
|  Squared Error   | $\frac{1}{2} (y^{(i)} - f(x^{(i)}))^2$ |                 $y^{(i)} - f(x^{(i)})$                 |                 E[y\|x(i)]                 |    L2Boosting     |
|  Absolute Error  |   $\vert y^{(i)} - f(x^{(i)}) \vert$   |              $sign(y^{(i)} - f(x^{(i)})$               |         $median(y \vert x^{(i)})$          | **Gradient Boosting** |
| Exponentail Loss | $\exp(- \tilde {y^{(i)}} f(x^{(i)}))$  | $- \tilde {y^{(i)}} exp(-\tilde {y^{(i)}} f(x^{(i)}))$ | $\frac{1}{2} \log \frac{\pi_i}{1 - \pi_i}$ |     AdaBoost      |
|     LogLoss      |  $\log (1+e^{- \tilde{y^{(i)}} f_i})$  |                     y(i)−πiy(i)−πi                     | $\frac{1}{2} \log \frac{\pi_i}{1 - \pi_i}$ |    LogitBoost     |



#### **Forward Stagewise Additive Modeling 前向分步加法模型**

- 加法模型（Additive model）

  

  $f(x) = \sum_{k=1}^{K} \beta_k \cdot b(x; \gamma_k)$

  

  其中，b(x;γk) 为基函数，γk为基函数的参数，βk为基函数的系数。

  

- 前向分步算法

  在给定训练数据及损失函数L(y,f(x))的条件下，学习加法模型f(x)成为经验风险极小化即损失函数极小化的问题：

  $$\min_{\beta_k, \gamma_k} \quad \sum_{i=1}^{M} L \left[y^{(i)}, \sum_{k=1}^{K} \beta_k b(x^{(i)}; \gamma_k)\right]$$


  通常这是一个复杂的优化问题。forward stagwise algorithm求解这一优化问题的思路是：**因为学习的是加法模型，如果能够从前向后，每一步只学习一个基函数及其系数，逐步逼近优化目标函数式，那么就可以简化优化的复杂度**。具体地，每步只需优化如下损失函数：

  $$\min_{\beta, \gamma} \quad \sum_{i=1}^{M} L(y^{(i)}, \beta b(x^{(i)}; \gamma))$$

  给定训练数据集$D=\{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \cdots, (x^{(M)}, y^{(M)})\}, x^{(i)} \in \mathcal{X} \subseteq R^n, y^{(i)} \in \mathcal{Y} =$, 损失函数L(y,f(x))和基函数的集合{b(x;γ)}，学习加法模型f(x)f(x)的前向分步算法如下：

这样前向分步算法将**同时求解**从k=1到K的所有参数βk,γk的优化问题简化为**逐次求解**各个βk,γk的优化问题。

$\{ \\   \quad 输入：训练数据集D=\{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \cdots, (x^{(M)}, y^{(M)})\}; 损失函数L(y, f(x));\\   \qquad\quad  基函数集\{b(x, \gamma)\}； \\   \quad 输出：加法模型f(x)。 \\   \quad 计算过程：\\   \qquad (1). 初始化f_0(x) = 0 \\   \qquad (2). 对于k=1,2,\cdots,K \\   \qquad\qquad (a). 极小化损失函数 \\   \qquad\qquad\qquad (\beta_k, \gamma_k) = \arg \min_{\beta, \gamma} \sum_{i=1}^{M} L(y^{(i)}, f_{k-1}(x^{(i)}) + \beta b(x; \gamma)) \quad(n.ml.1.6.2)\\   \qquad\qquad 得到参数\beta_k, \gamma_k. \\   \qquad\qquad (b). 更新 \\   \qquad\qquad\qquad f_k(x) = f_{k-1}(x) + \beta_k b(x; \gamma_k) \qquad(n.ml.1.6.3) \\   \qquad (3). 得到加法模型 \\   \qquad\qquad\qquad f(x) = f_K(x) = \sum_{k=1}^{K} \beta_k b(x; \gamma_k) \qquad(n.ml.1.6.4) \\   \}$

二分类问题时损失函数示意图：



[![损失函数示意图](https://raw.githubusercontent.com/ComputationalAdvertising/spark_lr/master/img/ml_6_1_1_loss_function_graph.png)](https://raw.githubusercontent.com/ComputationalAdvertising/spark_lr/master/img/ml_6_1_1_loss_function_graph.png)



### **Adaboost**

$\begin{align}     &\{ \\     &\quad 输入：训练数据集D=\{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \cdots, (x^{(M)},y^{(M)})\}，x^{(i)} \in \mathcal{X} \subseteq R^N，\\     &\qquad\qquad y^{(i)} \in \mathcal{Y} = \{-1, +1\}, 弱分类器; \\     &\quad 输出：最终分类器G(x). \\     &\quad 过程：\\     &\qquad (1). 初始化训练数据的权值分布 \\     &\qquad\qquad\qquad D_1=(w_{11}, w_{12}, \cdots, w_{1M}), \quad w_{1i}=\frac{1}{M}, \; i=1,2,\cdots,M \\     &\qquad (2). 训练K个弱分类器 k=1,2,\cdots,K \\     &\qquad\qquad (a). 使用具有权值分布D_k的训练数据集学习，得到基本分类器 \\     &\qquad\qquad\qquad\qquad G_k(x): \mathcal{X} \rightarrow \{-1, +1\} \qquad\qquad(ml.1.6.3)\\     &\qquad\qquad (b). 计算G_k(x)在训练数据集上的分类误差率 \\     &\qquad\qquad\qquad\qquad e_k = P(G_k(x^{(i)}) \not= y^{(i)}) = \sum_{i=1}^{M} w_{ki} I(G_k(x^{(i)}) \not= y^{(i)}) \qquad(ml.1.6.4)\\     &\qquad\qquad (c). 计算G_k(x)的系数 \\     &\qquad\qquad\qquad\qquad \alpha_k = \frac{1}{2} \log \frac{1-e_k}{e_k} \quad(e是自然对数) \qquad(ml.1.6.5)\\     &\qquad\qquad (d). 更新训练数据集的权值分布 \\     &\qquad\qquad\qquad\qquad D_{k+1} = (w_{k+1,1}, w_{k+1,2}, \cdots, w_{k+1,M})\\     &\qquad\qquad\qquad\qquad w_{k+1,i} = \frac{w_{k,i}}{Z_k} \exp(-\alpha_k y^{(i)} G_k(x^{(i)})), \quad i=1,2,\cdots,M \quad(ml.1.6.6)\\     &\qquad\qquad\qquad Z_k是规范化因子 \\     &\qquad\qquad\qquad\qquad Z_k = \sum_{i=1}^{M} w_{k,i} \cdot \exp(-\alpha_k y^{(i)} G_k(x^{(i)})) \qquad(ml.1.6.7)\\     &\qquad\qquad\qquad 使D_{k+1}成为一个概率分布。\\     &\qquad (3). 构建基本分类器的线性组合 \\     &\qquad\qquad\qquad f(x) = \sum_{k=1}^{K} \alpha_k G_k(x) \qquad(ml.1.6.8)\\     &\qquad 得到最终的分类器 \\     &\qquad\qquad G(x) = sign (f(x)) = sign \left(\sum_{k=1}^{K} \alpha_k G_k(x) \right) \qquad(ml.1.6.9)\\     &\}     \end{align}$

<img src="https://i.loli.net/2019/10/18/fwCHZ6gzMP1nTXv.png" alt="image-20191009003432594.png" style="zoom: 33%;" />

- Setting: Classification (yi∈{+1,−1}
- Weak learners: h∈H are binary, h(xi)∈{−1,+1},∀x
- Step-size: We perform line-search to obtain best step-size α
- Loss function: Exponential loss $\ell(H)=\sum_{i=1}^{n} e^{-y_i H(\mathbf{x}_i)}$

#### Finding the best weak learner

math detail go to http://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote19.html



#### AdaBoost Pseudo-code

![image-20191009004023929.png](https://i.loli.net/2019/10/18/SYBzwrvDEUbNtqs.png)

A few remarks:

- As long as H is negation closed (this means for every h∈H we must also have −h∈H), it cannot be that the error ϵ>1/2. The reason is simply that if h has error ϵ, it must be that −h has error 1−ϵ. So you could just flip h to −h and obtain a classifier with smaller error. As hhwas found by minimizing the error, this is a contradiction.
- The inner loop can terminate as the error ϵ=1/2, and in most cases it will converge to 1/2 over time. In that case the latest weak learner h is only as good as a coin toss and cannot benefit the ensemble (therefore boosting terminates). Also note that if ϵ=1/2 the step-size α would be zero.



**AdaBoost算法描述说明**

1. 假设训练数据集具有相同的权值分布，每个训练样本在基本分类器的学习中作用相同。这一假设保证，第一步能在原始数据上学习基本分类器G1(x)

2. AdaBoost反复学习基本分类器，在每一轮k=1,2,⋯,K顺序地执行下列操作：

   

   1) 学习基本分类器：使用当前分布Dk加权的训练数据集，学习基本分类器Gk(x)

   

   2) 误差率：计算基本分类器Gk(x)在加权训练数据集上的分类误差率

$e_k = P(G_k(x^{(i)}) \not= y^{(i)}) = \sum_{G_k(x^{(i)}) \not= y^{(i)}} w_{ki}$		

wki表示第k轮中第i个样本的权值，$\sum_{i=1}^{M} w_{ki} = 1$

这表明Gk(x)在加权的训练数据集上的分类误差率是Gk(x)误分类样本的权值之和。

​		3) 分类器权重：计算基本分类器Gk(x)的系数αk，αk表示Gk(x)在最终分类器中的重要性。当ek≤0.5时，αk≥0，并且αk随着ek的减小而增大，所以分类误差率越小的基本分类器在最终分类器中的作用越大。

​		4) 更新训练数据的权值分布，为下一轮做准备，

$$w_{k+1, i} =   \begin{cases}   \frac{w_{ki}} {Z_k} e^{-\alpha_k}, &\quad G_k(x^{(i)}) = y^{(i)} \\   \frac{w_{ki}} {Z_k} e^{\alpha_k}, &\quad G_k(x^{(i)}) \not= y^{(i)}   \end{cases}$$

由此可知，被基本分类器Gk(x)误分类样本的权值得以扩大，而被正确分类样本的权值却得以缩小。相比较来说，误分类样本的权值被放大$e^{2\alpha_k} = \frac{e_k}{1-e_k}$倍。因此，错误分类样本在下一轮学习中起更大的作用

**不改变所给的训练数据，而不断改变训练数据权值的分布，使得训练数据在基本分类器中起不同的作用，这也是AdaBoost的一个特点。**

3. 线性组合f(x)实现K个基本分类器的加权表决。系数αk表示了分类器Gk(x)的重要性。

注意：在这里所有αk之和并不为1。f(x)的符号决定实例x的类别，f(x)的绝对值表示分类的精确度。

**利用基本分类器的线性组合构建最终分类器是AdaBoost的另一个特点。**



#### **训练误差分析**

**AdaBoost算法最基本的性质是它能在学习过程中不断减少训练误差，即在训练数据集上的分类误差率。** 对于这个问题，有个定理可以保证分类误差率在减少－AdaBoost的训练误差界。

- **定理：AdaBoost训练误差界**

$$\frac{1}{M} \sum_{i=1}^{M} I(G(x^{(i)}) \not= y^{(i)}) \le \frac{1}{M} \sum_{i} \exp(-y^{(i)} f(x^{(i)})) = \prod_{k=1}^{K} Z_k \qquad(ml.1.6.10)$$

> G(x(i))≠y(i)时，y(i)f(x(i))<0，因而$\exp(-y^{(i)} f(x^{(i)})) \ge 1$。由此可以直接推导出前半部分。

> 后半部分的推导要用到Zk的定义式(ml.1.6.7)(ml.1.6.7)和(ml.1.6.6)(ml.1.6.6)的变形:

> wk,iexp(−αky(i)Gk(x(i)))=Zkwk+1,i

> 推导如下：
>
> $$\begin{align}     \frac{1}{M} \sum_{i=1}^{M} \exp(-y^{(i)} f(x^{(i)})) &= \underline{ \frac{1}{M} \sum_{i=1}^{M} } \exp \left(-\sum_{k=1}^{K} \alpha_k y^{(i)} G_k(x^{(i)}) \right) \\     &= \underline{ \sum_{i=1}^{M} w_{1,i} } \prod_{k=1}^{K} \exp(-\alpha_k y^{(i)} G_k(x^{(i)})) \\     &= \sum_{i=1}^{M} w_{1,i} \underline{ \exp(-\alpha_1 y^{(i)}) G_k(x^{(i)}) \prod_{k=2}^{K} \exp(-\alpha_k y^{(i)} G_k(x^{(i)})) } \\     &= Z_1 \sum_{i=1}^{M} w_{2,i} \prod_{k=2}^{K} \exp(-\alpha_k y^{(i)} G_k(x^{(i)})) \\     &= Z_1 Z_2 \sum_{i=1}^{M} w_{3,i} \prod_{k=3}^{K} \exp(-\alpha_k y^{(i)} G_k(x^{(i)})) \\     &= \cdots \cdots \\     &= Z_1 Z_2 \cdots Z_{K-1} \sum_{i=1}^{M} w_{K,i} \exp(-\alpha_K y^{(i)} G_K(x^{(i)})) \\     &= \prod_{k=1}^{K} Z_k     \end{align} \quad(n.ml.1.6.8)$$

> 注意：$w_{1,i} = \frac{1}{M}$

这一定理说明：**可以在每一轮选取适当的Gk使得Zk最小，从而使训练误差下降最快。** 对于二类分类问题，有如下定理。

- 定理：二类分类问题AdaBoost训练误差界

  $$\prod_{k=1}^{K} Z_k = \prod_{k=1}^{K} \left[2 \sqrt{e_k(1-e_k)} \;\right] = \prod_{k=1}^{K} \sqrt{(1-4\gamma_k^2)} \le \exp \left(-2 \sum_{k=1}^{K} \gamma_k^2 \right) \qquad(ml.1.6.9)$$

$\gamma_k = 0.5 - e_k$

> 证明：由公式(ml.1.6.7)(ml.1.6.7)和(n.ml.1.6.5)(n.ml.1.6.5)可得：
>
> 
>
> Zk=∑i=1Mwk,iexp(−αky(i)Gk(x(i)))=∑y(i)=Gk(x(i))wk,i⋅e−αk+∑y(i)≠Gk(x(i))wk,i⋅eαk=(1−ek)⋅e−αk+ek⋅eαk=2ek(1−ek)‾‾‾‾‾‾‾‾‾√=1−4γ2m‾‾‾‾‾‾‾‾√(n.ml.1.6.9)Zk=∑i=1Mwk,iexp⁡(−αky(i)Gk(x(i)))=∑y(i)=Gk(x(i))wk,i⋅e−αk+∑y(i)≠Gk(x(i))wk,i⋅eαk=(1−ek)⋅e−αk+ek⋅eαk=2ek(1−ek)=1−4γm2(n.ml.1.6.9)
>
> 
>
> 注：αk=12log1−ekek,eαk=1−ekek‾‾‾‾√αk=12log⁡1−ekek,eαk=1−ekek
>
> 对于不等式部分
>
> 
>
> ∏k=1K1−4γ2m‾‾‾‾‾‾‾‾√≤exp(−2∑k=1Kγ2k)(n.ml.1.6.10)∏k=1K1−4γm2≤exp⁡(−2∑k=1Kγk2)(n.ml.1.6.10)
>
> 则可根据exex和1−x‾‾‾‾‾√1−x在点x=0x=0的泰勒展开式推出不等式1−4γ2m‾‾‾‾‾‾‾‾√≤exp(−2γ2m)1−4γm2≤exp⁡(−2γm2)。



**AdaBoost训练误差指数速率下降**

$$\frac{1}{M} \sum_{i=1}^{M} I(G(x^{(i)}) \not= y^{(i)}) \le \exp(-2K\gamma^2) \qquad(ml.1.6.12)$$

推论表明，在此条件下，**AdaBoost的训练误差是以指数速率下降的**。这一性质对于AdaBoost计算（迭代）效率是利好消息。



- AdaBoost优点 

（1）是一种有很高精度的分类器
（2）可以使用各种方法构建子分类器，Adaboost算法提供的是框架
（3）当使用简单分类器时，计算出的结果是可以理解的，并且弱分类器的构造极其简单
（4）简单，不用做特征筛选
（5）不容易发生overfitting。

- AdaBoost算法缺点
  - 对异常点敏感
  - 指数损失存在的一个问题是不断增加误分类样本的权重（指数上升）。如果数据样本是异常点（outlier），会极大的干扰后面基本分类器学习效果；
  - 模型无法用于概率估计



## **Gradient Boosting algorithm**

https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d

Gradient boosting is a more generalized framework than adaboosting, because it has a error measurement, 

- Target variable  keeps changing, **residuals** instead of **y**
- cumulative decision boundary is used 



The objective of any supervised learning algorithm is to define a loss function and minimize it. Let’s see how maths work out for Gradient Boosting algorithm. Say we have mean squared error (MSE) as loss defined as:

![image.png](https://i.loli.net/2019/10/09/b41WnlyxdHQk2FE.png)



We want our predictions, such that our loss function (MSE) is minimum. By using **gradient descent** and updating our predictions based on a learning rate, we can find the values where MSE is minimum.

![image.png](https://i.loli.net/2019/10/09/A3cGVfZB6tvUyO4.png)



So, we are basically updating the predictions such that the sum of our residuals is close to 0 (or minimum) and predicted values are sufficiently close to actual values.

提升树方法是利用加法模型与前向分布算法实现整个优化学习过程, Adaboost的指数损失和回归提升树的平方损失，在前向分布中的每一步都比较简单。但对于一般损失函数而言（比如绝对损失），每一个优化并不容易。针对这一问题。Freidman提出了梯度提升（gradient boosting）算法。该算法思想：

**利用损失函数的负梯度在当前模型的值作为回归问题提升树算法中残差的近似值，拟合一个回归树。**

损失函数的负梯度为：

$$-\left[ \frac{\partial L(y^{(i)}, f(x^{(i)}))} {\partial f(x^{(i)})} \right]_{f(x) = f_{k-1}(x)} \approx r_{m,i}$$


- Gradient Boosting－算法描述

$\{ \\   \quad\, 输入：训练数据集D=\{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \cdots, (x^{(M)}, y^{(M)})\}, x^{(i)} \in \mathcal{X} \subseteq R^n, y^{(i)} \in \mathcal{Y}; \\   \qquad\quad\; 损失函数L(y, f(x)); \\   \quad 输出：提升树\hat{f}(x). \\   \quad 过程: \\   \qquad (1). 初始化模型 \\   \qquad\qquad\qquad f_0(x) = \arg \min_c \sum_{i=1}^{M} L(y^{(i)}, c)； \\   \qquad\; (2). 循环训练K个模型 k=1,2,\cdots,K \\   \qquad\qquad (a). 计算残差：对于i=1,2,\cdots,M \\   \qquad\qquad\qquad\qquad r_{ki} = -\left[ \frac{\partial L(y^{(i)}, \; f(x^{(i)}))} {\partial f(x^{(i)})} \right]_{f(x) = f_{k-1}(x)} \\   \qquad\qquad (b). 拟合残差r_{ki}学习一个回归树，得到第k颗树的叶结点区域R_{kj}，\quad j=1,2,\cdots,J \\   \qquad\qquad (c). 对j=1,2,\cdots,J, 计算：\\   \qquad\qquad\qquad\qquad c_{kj} = \arg \min_c \sum_{x^{(i)} \in R_{kj}} L(y^{(i)}, \; f_{k-1}(x^{(i)}) + c)\\   \qquad\qquad (d). 更新模型：\\   \qquad\qquad\qquad\qquad    f_k(x) = f_{k-1}(x) + \sum_{j=1}^{J} c_{kj} I(x \in R_{kj}) \\   \qquad\; (3). 得到回归提升树 \\   \qquad\qquad\qquad \hat{f}(x) = f_K(x) = \sum_{k=1}^{K} \sum_{j=1}^{J} c_{kj} I(x \in R_{kj}) \\    \}$;


  > 算法解释：
  > 1. 第（1）步初始化，估计使损失函数极小化的常数值（是一个只有根结点的树）；
  > 2. 第(2)(a)步计算损失函数的负梯度在当前模型的值，将它作为残差的估计。(对于平方损失函数，他就是残差；对于一般损失函数，它就是残差的近似值)
  > 3. 第(2)(b)步估计回归树的结点区域，以拟合残差的近似值；
  > 4. 第(2)(c)步利用线性搜索估计叶结点区域的值，使损失函数极小化；
  > 5. 第(2)(d)步更新回归树。





------

第一个，GDBT。

对于这个，一旦对上面梯度提升的想法理解了那就很容易解释了。首先既然是树，那么它的基函数肯定就是决策树啦，而损失函数则是根据我们具体的问题去分析，但方法都一样，最终都走上了梯度下降的老路，比如说进行到第m步的时候，首先计算残差

<img src='https://img-blog.csdn.net/20170309121122851?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

有了残差之后，我们再用（xi,rim）去拟合第m个基函数，假设这棵树把输入空间划分成j个空间R1m，R2m……，Rjm，假设它在每个空间上的输出为bjm，这样的话，第m棵树可以表示如下：

<img src='https://img-blog.csdn.net/20170309121147649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

下一步，对树的每个区域分别用线性搜索的方式寻找最佳步长，这个步长可以和上面的区域预测值bjm进行合并，最后就得到了第m步的目标函数

<img src='https://img-blog.csdn.net/20170309121209540?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

当然了，对于GDBT比较容易出现过拟合的情况，所以有必要增加一点正则项，比如叶节点的数目或叶节点预测值的平方和，进而限制模型复杂度的过度提升，这里在下面的实践中的参数设置我们可以继续讨论。

第二个，AdaBoost。

首先要说的是是AdaBoost是用于分类的。然后套路想必你已经非常了解了，前面几步完全和上面的GDBT一样，区别在于AdaBoost给出了损失函数为指数损失函数，即

<img src='https://img-blog.csdn.net/20170309121233561?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

很好理解，预测正确了yf(x)为正值，损失函数值就小，预测错误yf(x)为正值，损失函数值较大，然后我们来看一下第m步的损失函数
<img src='https://img-blog.csdn.net/20170309121256760?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

现在就是分别求alpha和G（x）使得损失函数最小值，按照之前的想法，直接算伪残差然后用G（x）拟合，不过这边我们先不着急。指数项中，yi与fm-1的乘积是不依赖于alpha和G（x）的，所以可以提出来不用考虑，对于任意alpha>0，在exp(-yi*fm-1)权值分布下，要exp(-yi*alpha*G(x))取最小值，也就是要G（x）对加权y预测的正确率最高。接下来，求alpha很愉快，直接求导位0，懒癌发作，公式推导过程就不打了，最后的结果如下：




得到了参数之后就能愉快的迭代，使得训练数据上的正确率蹭蹭蹭地往上涨。

再回过头来看看AdaBoost的标准做法和我们推导的是否一致

1 第一步假设平均分布，权值都为1/N，训练数据得到分类器。

2 求第一步的分类器预测数据的错误率，计算G（x）的系数alpha。

3 更新权值分布，不过加了归一化因子，使权值满足概率分布。

4 基于新的权值分布建立新的分类器，累加在之前的模型中。

5 重复上述步骤，得到最终的分类器。

可以看出，除了在更新权值分布处加了一个归一化因子之外，其他的都和我们推导的一样，所以，所以什么呀……你不仅会用还会推导啦？O(∩_∩)O哈哈~

第三个，XGBoost。

其实说白了也很简单，之前用的梯度下降的方法我们都只考虑了一阶信息，根据泰勒展开，

我们可以把二阶信息也用上，假如目标函数如下

<img src='https://img-blog.csdn.net/20170309121411015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

啊啊啊，这公式打得我真要吐血了。其中Ω为正则项，正如上面讲的，可表示如下
<img src='https://img-blog.csdn.net/20170309121507078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>


然后对于决策树而言，最重要的就是一共有多少个节点以及每个节点的权值，所以决策树可以表示为

<img src='https://img-blog.csdn.net/20170309121521078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

这样就有了下一步的推导

<img src='https://img-blog.csdn.net/20170309121540498?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>



第二步是因为不管fm如何取值第一项的值都不变，所以优化过程中可以不用考虑，第三步是因为对于每个样本而言其预测值就是对应输入空间对应的权值，第四步则是把样本按照划分区域重新组合，然后定义
<img src='https://img-blog.csdn.net/20170309121604609?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

带入对w求偏导使其为0，这样就求得了

<img src='https://img-blog.csdn.net/20170309121623874?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

再回代，就可以把J（fm）中的w给消去了，得到了
<img src='https://img-blog.csdn.net/20170309121644250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMjI1OTQzMDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center'>

这样我们就把新一步函数的损失函数变成了只与上一步相关的一个新的损失函数，这样我们就可以遍历数据中所有的分割点，寻找新的损失函数下降最多的分割点，然后重复上述操作。

相比于梯度下降提升，XGBoost在划分新的树的时候还是用了二阶信息，因此能够更快地收敛，而且XGBoost包是用C/C++写的，所以速度更快，而且在寻找最佳分割点的时候，可以引入并行计算，因此速度进一步提高，广受各大竞赛参赛者的喜爱啊。


