Clipped from: https://www.thelearningmachine.ai/lle





> 准确是两个概念。准是 bias 小，确是 variance 小。准确是相对概念，因为 bias-variance tradeoff。
> ——Liam Huang



**Regularization increases Bias and reduces Variance**



L1 penalty / Penalty Term / Regularisation Term



Fit training data well (OLS)

Keep parameters small

A trade-off between fitting the training data well and keeping parameters small

The LASSO method puts a constraint on the sum of the absolute values of the model parameters, the sum has to be less than a fixed value (upper bound, or t):

#  

#  

》 In order to do so, the method applies a shrinking (regularization) process where it penalizes the coefficiets of the regression variables shrinking some of them to zero. During features selection process the variables that still have a non-zero coefficient after the shrinking process are selected to be part of the model. The goal of this process is to minimize the prediction error.

In practice, the tuning parameter α that controls the strength of the penalty assumes great importance. Indeed, when α is sufficiently large, coefficients are forced to be exactly equal to zero. This way, dimensionality can be reduced. The larger the parameter α, the more the number of coefficients are shrunk to zero. On the other hand, if α = 0, we have just an OLS (Ordinary Least Squares) regression.


