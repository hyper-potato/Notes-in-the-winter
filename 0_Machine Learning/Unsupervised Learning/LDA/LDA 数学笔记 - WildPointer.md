[yanshengjia.com](https://yanshengjia.com/2017/12/16/LDA-数学笔记/ "Malformed request")

# LDA笔记

**Latent**: everything that we don’t know a priori and are hidden in the data. Here, the themes or topics that document consists of are unknown, but they are believed to be present as the text is generated based on those topics

**Dirichlet:** It is a ‘distribution of distributions’.  In the context of topic modeling, the Dirichlet is the distribution of topics in documents and distribution of words in the topic. 

**Allocation**: This means that once we have Dirichlet, we will **allocate topics to the documents** and **words** of the document **to topics.**





**文档是主题上的概率分布，主题是单词上的概率分布。**

LDA 接受的输入是若干文档。她认为每篇文档中的单词是语义相关的。她试图想明白每篇文档是如何创造出来的。我们只需要告诉她有多少个主题需要构造，她就会根据主题的数量在语料库上生成主题和单词的分布。基于她的输出，可以预测新文档的主题分布，计算文档之间的相似度，文本分类。

LDA

- Input
  - 文档集 (Corpus) bag-of-words: word order is discarded 
  - K: the number of topics
- Output
  - 主题的单词分布 (Topic - Word Distribution) -> 每个主题在讨论什么
  - 文档的主题分布 (Doc - Topic Distribution) -> 每篇文档中包含了哪些主题

优点

- Realistically models multiple topics/document 
- Easily generalizable to unseen documents 
- 对于每一个主题均可找出一些词语来描述它
- 是一个有效的主题建模工具
- 在许多领域已经展示出了很好的结果，比如推荐，文本分类

不足

- 必须事先指定主题的数量 K，而这个 K 的选取是困难的，需要大量的试验来确定
- Bag-of-words representation is unrealistic 

- 假如语料库中有许多关联的主题，狄利克雷主题分布无法捕捉主题间的相关性





LDA (Latent Dirichlet Allocation) 通过建立一种

- 词袋模型 (Bag-of-Word Model)：一篇文档是由一组词构成的一个集合，词与词之间没有顺序以及先后的关系
- 统计模型 (Statistical Model)：经过数理统计法求得各变量之间的函数关系
- 生成概率模型 (Generative Probabilistic Model)：认为每一类数据都服从某一种分布，如狄利克雷分布
- 主题模型 (Topic Model)：在机器学习和自然语言处理等领域，用来在一系列文档中发现抽象主题的一种统计模型
- 无监督学习模型 (Unsupervised Model)：在训练时不需要人工标注的数据集，需要的仅仅是文档集以及指定主题的数量 K

来发现文档集 (Document Collection / Corpus) 中的抽象主题，从而试图解决这类问题。



## 直觉 Intuition behind LDA

<img src="https://raw.githubusercontent.com/yanshengjia/photo/master/lda0.png" style="zoom:50%;" />

现在我们有六篇文档，如上图所示，前三篇的主题是关于水果的，后三篇是关于动物的。

假如将这六篇文档作为 LDA 模型的输入，并且设 K = 2，这里的 K 是 LDA 模型中的一个重要参数，代表我们想从文档集中抽取出的主题的数量。

LDA 的第一个输出是主题的单词分布 (Topic - Word Distribution)，由上图可知，主题1的单词概率分布是 apple banana orange 各占 33%，这就是 LDA 描述主题的方法，她使用单词 (Word / Term) 来描述主题。

LDA 的第二个输出是文档的主题分布 (Doc - Topic Distribution)，由上图可知，文档1中单词 apple banana 都是关于水果的，所以它的主题是 100% 关于主题1 (水果)，文档4的主题是 100% 关于主题2 (动物)。当然，一个文档中是可以包含多个主题的，此处的例子只包含一个主题是因为文档中单词比较少，正好都关于一个主题。

让我们梳理一下，LDA 的输入是一个文档集，输出是2个分布，基本上她会告诉你 **每个主题在讨论什么** 以及 **每篇文档中包含了哪些主题**。



## 贝叶斯定理 Bayes Theorem

$P(A|B)=\frac{P(A)\times{P(B|A)}}{P(B)}$

$posterior = prior \times likelihood$

后验概率=先验概率×似然

- 概率: 用于在已知一些参数的情况下，预测接下来的观测所得到的结果。
- 似然: 用于在已知某些观测所得到的结果时，对有关事物的性质的参数进行估计。
- P(A|B): 是已知 B 发生后 A 的条件概率，也由于得自B的取值而被称作 A 的后验概率。
- P(A): 是 A 的先验概率。之所以称为”先验”是因为它不考虑任何 B 方面的因素。
- P(B|A): 是已知 A 发生后 B 的条件概率，也由于得自A的取值而被称作 B 的后验概率。
- P(B): 是 B 的先验概率。
- P(B|A)P(B): 称作标准似然 (standardised likelihood)

举个好人坏人的例子，你对好人和坏人的认知，先验分布为：100个好人和100个的坏人，即你认为好人坏人各占一半，现在你被2个好人帮助了和1个坏人骗了 (似然)，于是你得到了新的后验分布为：102个好人和101个的坏人。现在你的后验分布里面认为好人比坏人多了。这个后验分布接着又变成你的新的先验分布，当你被1个好人帮助了和3个坏人骗了后 (似然)，你又更新了你的后验分布为：103个好人和104个的坏人。依次继续更新下去。

LDA 是基于贝叶斯模型的，她是一个三层贝叶斯模型。



## 伽玛函数 Gamma Function

$$\Gamma(x)=\int_0^{\infty}t^{x-1}e^{-t}dt$$

分部积分后，可以发现 Gamma 函数具有这样神奇的性质：

$$\Gamma(x+1) = x \Gamma(x)$$

于是很容易证明，`Gamma(x)`函数可以当成是阶乘在实数集上的延拓，具有如下性质

$$\Gamma(n) = (n-1)! $$



## 一些概率论 Some Probability Theory

### 二项分布 Binomial Distribution

N 重伯努利分布，X ~ B(n, p).

概率密度公式为：

$P(K = k) =  \begin{pmatrix}  n \\  k \\  \end{pmatrix} p^k{(1-p)}^{n-k}$

### 多项分布 Multinomial Distribution

多项分布，是二项分布扩展到多维的情况. 多项分布是指单次试验中的随机变量的取值不再是 0-1 的，而是有多种离散值可能（1,2,3…,k）. 概率密度函数为：

$P(x_1, x_2, ..., x_k; n, p_1, p_2, ..., p_k) = \frac{n!}{x_1!...x_k!}{p_1}^{x_1}...{p_k}^{x_k}$



### 贝塔分布 Beta Distribution

Beta分布的定义：对于参数 α>0,β>0 , 取值范围为 [0, 1] 的随机变量 X 的概率密度函数为：

$f(x; \alpha, \beta) = \frac{1}{B(\alpha, \beta)} x^{\alpha - 1} {(1-x)}^{\beta-1}$

where $\frac{1}{B(\alpha, \beta)} = \frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}$

### 共轭分布 Conjugate Distribution

在贝叶斯理论中，如果后验概率分布 P(θ|x) 与先验概率分布 P(θ) 具有相同形式，则先验分布与后验分布被称为共轭分布，先验分布被称为似然函数的共轭先验。

$P(\theta | x) = \frac{P(\theta, x)} {P(x)}$

共轭先验的好处主要在于代数上的方便性，可以直接给出后验分布的封闭形式，否则的话只能数值计算。共轭先验也有助于获得关于似然函数如何更新先验分布的直观印象。

为什么要讲共轭分布呢？

联系到前文提到的贝叶斯理论，因为我们希望先验分布和似然对应的后验分布，在后面还可以作为先验分布！就像上面例子里的 “102个好人和101个的坏人” ，它既是前面一次贝叶斯推荐的后验分布，又是后一次贝叶斯推荐的先验分布。换句话说，我们希望先验分布和后验分布的形式是一样的，这样的分布叫共轭分布。

根据 二项分布、贝塔分布、多项式分布、狄利克雷分布的公式，数学上可以推导出：

贝塔分布是二项式分布的共轭先验分布，**狄利克雷分布是多项式分布的共轭先验分布**。



## 隐含狄利克雷分布主题模型 LDA Topic Model

### 使用场景 Use Cases

- 自动推测出文档集中讨论的主题
- 推测出的主题信息可以用于总结和组织文档，也可以用于降低特征 (featurization) 和维度 (dimensionality)
  - 文档 X 在讨论什么？-> 主题识别
  - 文档 X 和 文档 Y 的相似度是多少？-> 文本相似度计算
  - 如果我对主题 Z 感兴趣，那么哪篇文档最对我的胃口？ -> 推荐系统
- 其他使用场景
  - 情感分析 Sentiment analysis
  - 图片目标定位 Object localization for images
  - 音乐自动谐波分析 Automatic harmonic analysis for music
  - 生物信息学 Bioinformatics
  - analyze every GitHub repository’s topics/tags and infer themes like native desktop client, back-end web service, single-paged app, or flappy bird clone.
  - Summarizing Collections of Images



## 假设 Assumptions

- Documents with similar topics will use similar groups of words
- Document definitions / modeling:
  - Documents are probability distributions over latent topics 文档是潜在主题的概率分布
  - Topics are probability distributions over words 主题是单词的概率分布

Doc - Topic Distribution

<img src="https://raw.githubusercontent.com/yanshengjia/photo/master/lda2.png" style="zoom:50%;" />



Topic - Word Distribution

<img src="https://raw.githubusercontent.com/yanshengjia/photo/master/lda3.png" style="zoom:50%;" />
LDA 认为单词携带了强烈的语义信息，包含相似主题的文档中会出现相似的单词组。

一篇文档可以包含多个主题，文档中每一个词都由其中的一个主题生成，每个主题都有一个单词的概率分布与之相关联。

**LDA 模型与别的词袋模型的不同之处就在于，**它利用了单词的概率分布而不是单词的出现频率。其他词袋模型可能会考虑文档中出现得最频繁的单词，而 LDA 使用了一种更全面的方法：**考虑主题的单词分布。**



### 盘子表示法 Plate Notation

LDA 是一个三层贝叶斯模型，如下图所示，矩形框是“盘子”，代表复制品 (replicates)。

- 大盘子代表文档层
- 中盘子代表单词层
- 小盘子代表主题层

根据这些参数指向的位置 (落在哪个盘子中)，可以看出一个参数是应用于文档层，还是应用于主题层，还是单词层。Main variables of interest:

– $\beta_{k}$: distribution over vocabulary for topic k

– $θ_{d,k}$ : topic proportion for topic k in document d

![](https://miro.medium.com/max/988/1*VTHd8nB_PBsDtd2hd87ybg.png)

- M: 语料库中文档的数量

- N: 文档中单词的数量

- K: 语料库中主题的数量

- α is the per-document topic distributions, 每篇文档的主题分布 (先验狄利克雷分布) 的参数，是一个 K 维向量，K是主题数量
  - 越高，代表每篇文档可能包含更多的主题，而不只是包含一个或者两个特定的主题
  - 越低，代表每篇文档包含的主题数越少

- β is the per-topic word distribution, 每个主题的单词分布 (先验狄利克雷分布) 的参数，是一个 V 维向量，V 是文档中单词的数量
  - 越高，代表每个主题可能包含更多的单词
  - 越低，代表每个主题包含的单词数越少

- θ is the topic distribution for document m, 文档m的主题分布
- φ is the word distribution for topic k, 主题k的单词分布
- z is the topic for the *n*-th word in document *m*, $z_{ij}$文档m中第 n个单词的主题编号，服从多项式分布
- w is the specific word,特定的单词，服从多项式分布



### 生成过程 Generative Process

为了能比较好地理解 LDA 推测文档的主题分布的过程，先了解一篇文档的生成过程是很有帮助的。

LDA 认为一篇新文档是以这样的方式生成的：

- For each document i, draw  $\theta_i \sim Dirichlet(\alpha), d = 1…D$
- For each word j in document i
  - Sample from $\theta_i$,draw a topic index, $z_{ij} \sim Multinomial(\theta_i)$
  - Sample from $\varphi*{z_{ij}}$,draw the observed word,$w_{ij} \sim Multinomial(\varphi*{z_{ij}})$

For parameter estimation, the posterior distribution is

$P(z, \theta, \varphi | w, \alpha, \beta) = \frac{P(z, \theta, \varphi | \alpha, \beta)}{P(w | \alpha, \beta)}$

简单点，一篇文档的生成分为三步：

- 决定文档中单词的数量
- 给文档选择一个主题分布 (i.e. 20% topic A, 30% topic B, 50% topic C)
- 生成文档中的单词，通过：
  - 首先基于文档的主题分布选一个主题
  - 再基于主题的单词分布随机选一个单词
  - 重复上面两步，直到单词数量达到一开始设定的上限



LDA 求解文档的主题分布实际上是上述生成过程的逆过程，从文档开始，反过来去发现文档中的主题。

- 生成过程：主题 -> 文档
- 逆过程：文档 -> 主题

## 逆过程 Working Backwards

假如你有一个语料库 / 文档集，你希望 LDA 去学习每篇文档中 k 个主题的分布，以及每个主题的单词分布。

LDA 从文档层回溯，去推测哪些主题可能用于生成该文档。

- 随机给每篇文档中的每个单词分配 (assign) k 个主题中的一个主题 Distribute these *k* topics across document *m* (this distribution is known as α and can be symmetric or asymmetric, more on this later) by assigning each word a topic.
- 对于每篇文档 d，更新 (重新采样 Resample) 单词的主题
  - 假设除了当前文档之外的所有主题分配是正确的 For each word *w* in document *m*, assume its topic is wrong but every other word is assigned the correct topic.
  - 计算两个概率
    - 当前文档 d 中的单词被分配到主题 t 的概率 P(topic t | doc d)
    - 所有文档中被分配到主题 t 的单词中是单词 w 的概率 P(word w | topic t)
  - 基于这两个概率的乘积 P(topic t | doc d) * P(word w | topic t) 分配给单词 w 一个新的主题
- 不断重复上述步骤，最终达到稳定状态，这时可以认为所有主题的分配是合理的，得到文档的主题分布

根据上文提到的文档生成模型，两个概率的乘积 P(topic t | doc d) * P(word w | topic t) 就是主题 t 生成单词 w 的概率，所以用这个概率重新采样当前单词的主题是说得通的。

这个逆过程就是吉布斯采样。



### 解法 Existing Solutions

有两种著名的算法，来估计 LDA 模型中的参数

- Variational EM (Expectation Maximization)
- Gibss Sampling

这两个算法都是批处理算法 (full batch algorithm)，运行时需要先把整个语料库读入内存，在每轮迭代中都会扫描 (scan) 语料库，这意味着巨大的内存压力。

当语料库很大的时候，批处理算法就显得力不从心了。如今，LDA 模型面对的输入语料库已经很大，比如 Wikipedia。













