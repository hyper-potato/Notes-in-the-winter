Clipped from: https://www.thelearningmachine.ai/lle

1. Introduction to Lasso Regularization Term (L1)

LASSO - Least Absolute Shrinkage and Selection Operator - was first formulated by Robert Tibshirani in 1996. It is a powerful method that performs two main tasks: regularization and feature selection.

Let's look at the example of lasso regularization with linear models, where OLS method is used with its reglarization term.

 L1 penalty / Penalty Term / Regularisation Term



Fit training data well (OLS)

Keep parameters small

A trade-off between fitting the training data well and keeping parameters small

The LASSO method puts a constraint on the sum of the absolute values of the model parameters, the sum has to be less than a fixed value (upper bound, or t):

#  

#  

In order to do so, the method applies a shrinking (regularization) process where it penalizes the coefficiets of the regression variables shrinking some of them to zero. During features selection process the variables that still have a non-zero coefficient after the shrinking process are selected to be part of the model. The goal of this process is to minimize the prediction error.

In practice, the tuning parameter α that controls the strength of the penalty assumes great importance. Indeed, when α is sufficiently large, coefficients are forced to be exactly equal to zero. This way, dimensionality can be reduced. The larger the parameter α, the more the number of coefficients are shrunk to zero. On the other hand, if α = 0, we have just an OLS (Ordinary Least Squares) regression.



There are many advantages of using the LASSO method.

1. First of all, it can provide a very good prediction accuracy, because shrinking and removing the coefficients can reduce variance without a substantial increase of the bias, this is especially useful when you have a small number of observation and a large number of features. In terms of the tuning parameter α we know that bias increases and variance decreases when α increases, indeed a trade-off between bias and variance has to be found.
2. Moreover, the LASSO helps to increase the model interpretability by eliminating irrelevant variables that are not associated with the response variable, this way also overfitting is reduced. This is the point where we are more interested in because in this paper the focus is on the feature selection task.