# Panel Data



**Count for something we dont observe**

These are Models that Combine Cross-section and Time-Series Data.



Endogeniety issue in panel data



Time-invariant confound





because you have a panel, (time series and cross-sectional), the differences between units are called individual effects or heterogeneity. 

Panel data models acknowledge that different units behave differently by adding an individual heterogeneity term denoted to the pooled model as follows: 

There are two popular panel data models that account for individual heterogeneity in two different ways. 

The first model is called the fixed effects model and it assumes that the heterogeneity term f and the independent variables x are correlated. 

The second model is called the random effects model and it assumes that the heterogeneity term f and the independent variables x are not correlated.  

We do Hausma test to choose btw fixed effects and random effects model  

- Estimate the fixed effects model using the command: xtreg invest assets, fe 

- Store the results from Step 1 using the command: est store fixed 

- Estimate the random effects model using the command: xtreg invest assets, re 

- Finally generate the Hausman test statistic using the command hausman fixed  

  

The estimation of panel data models boils down to the choice between three estimators: 

1) The pooled model should be used when there is no individual heterogeneity in the model. 

2) When there is individual heterogeneity and it is not correlated with the independent variables of the model, the random effects model should be preferred. The Hausman test helps us decide whether this is the case or not. 

3) If the individual heterogeneity is correlated with the independent variables, the fixed effects model should be used







回归要控制住最重要的因素，但是如果有一些因素对Y有很重要的影响，你却没有办法获得其观察值，例如能力对于考试成绩的影响回归中，能力一定是一个重要变量，但是你却观察不到（所谓的智商等只是能力的一个侧面）。在panel中，将这种东西放到个体效应中处理，解决了不可观测变量的控制问题。

个体效应分为随机效应和固定效应。很早以前固定效应指那种个体效应不是一个随机变量，在回归中相当于作为截距项处理。随机效应是一个随机变量，有一个假设分布。随机效应就相当于将误差里面的一个成分分离出来，算是控制了不可观察因素，同时这个随机效应被假设为不同X相关。

但是在实际操作上，如何分辨个体效应的随机还是固定，一直存在争论。传统的观点是，如果你的截面中包含了整个样本空间，那么相当于没有随机问题了，就用固定效应，如果你的截面只是样本空间的一个子集，那么涉及到抽样问题，因此就用随机效应。例如全国31个省市，如果你用31个省市的面板做，则用固定效应，因为你的样本空间就是31，不存在随机抽样问题。如果你只有5个省的面板，那么就用随机效应。（这一点和前面chyshl 在2楼的链接里面的思想是一样的。只有5个省的数据，你要推知全国，就要用随机效应。）

不过，这种简单的处理方法很快就碰到了无法解释的事情。第一，5个省是随机抽样出来的吗？不是的话怎么能用随机效应的假设分布呢？第二，你怎么知道你的样本空间是什么？如果用了31个省市，就是全样本了嘛？如果你做跨国数据，同样用到这31个省市，你就只能用随机效应了，在更大的范围内，31就是一个抽样。那这31一会儿是随机，一会儿是非随机，显然逻辑上有问题。真相只能有一个。

为了解决这个问题，现在的计量经济学书上全部将传统的观点改成了技术上公认的一种做法：如果个体效应和X相关，用固定效应比随机效应好，如果不相关，那么用随机效应效果好，但是固定效应也能用。检验方法就是hausman检验。如果检验失败，建议采用固定效应。

OLS估计结果有什么用？如果你的模型中不存在上面所说的“不可观测变量”，那么就没有必要用个体效应模型，直接用OLS回归就可以了。如果存在不可观测变量，那么你的OLS估计是有偏的，也就是不可靠的，自然不能用。

总之，一句话，用什么假设得到什么结果，结果之间的选择在于你对你的模型有什么看法。有什么看法决定你用什么假设，最终决定你有什么结果。