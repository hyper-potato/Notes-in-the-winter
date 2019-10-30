##  KNN

**kk-nearest neighbors** ― The k-nearest neighbors algorithm, commonly known as k-NN, is a non-parametric approach where the response of a data point is determined by the nature of its kk neighbors from the training set. It can be used in both classification and regression settings.

*Remark: The higher the parameter k, the higher the bias, and the lower the parameter k, the higher the variance.*

<img src='https://stanford.edu/~shervine/images/k-nearest-neighbors.png'>

### Pros/Cons to K-NN

1. **K-NN is pretty intuitive and simple**: K-NN algorithm is very simple to understand and equally easy to implement. To classify the new data point K-NN algorithm reads through whole dataset to find out K nearest neighbors.

2. **K-NN has no assumptions**: K-NN is a non-parametric algorithm which means there are assumptions to be met to implement K-NN. Parametric models like linear regression has lots of assumptions to be met by data before it can be implemented which is not the case with K-NN.

3. **No** **Training Step:** (“lazy”) K-NN does not explicitly build any model, it simply tags the new data entry based learning from historical data. New data entry would be tagged with majority class in the nearest neighbor.

4. **It constantly evolves**: Given it’s an instance-based learning; k-NN is a memory-based approach. The classifier immediately adapts as we collect new training data. It allows the algorithm to respond quickly to changes in the input during real-time use.

5. **Very easy to implement for multi-class problem:** Most of the classifier algorithms are easy to implement for binary problems and needs effort to implement for multi class whereas K-NN adjust to multi class without any extra efforts.

6. **Can be used both for Classification and Regression:** One of the biggest advantages of K-NN is that K-NN can be used both for classification and regression problems.

7. **One Hyper Parameter:** K-NN might take some time while selecting the first hyper parameter but after that rest of the parameters are aligned to it.

8. Variety of distance criteria to be choose from: 

   K-NN algorithm gives user the flexibility to choose distance while building K-NN model.

   1. Euclidean Distance
   2. Hamming Distance
   3. Manhattan Distance
   4. Minkowski Distance



Even though K-NN has several advantages but there are certain very important disadvantages or constraints of K-NN. Below are listed few cons of K-NN.

1. **K-NN slow algorithm**: To determine the nearest neighbor of a new point x, must compute the distance to all m training examples. Run time performance is slow, but can be improved. 

   Improve:

   1. Pre-sort training examples into fast data structures 
   2. Compute only an approximate distance
   3. Remove redundant data (condensing) 

2. **Curse of Dimensionality:** KNN works well with small number of input variables but as the numbers of variables grow K-NN algorithm struggles to predict the output of new data point.

3. **K-NN needs homogeneous features**: If you decide to build k-NN using a common distance, like Euclidean or Manhattan distances, it is completely necessary that features have the same scale, since absolute differences in features weight the same, i.e., a given distance in feature 1 must means the same for feature 2.

4. **Optimal number of neighbors**: One of the biggest issues with K-NN is to choose the optimal number of neighbors to be consider while classifying the new data entry.

5. **Imbalanced data causes problems**: k-NN doesn’t perform well on imbalanced data. If we consider two classes, A and B, and the majority of the training data is labeled as A, then the model will ultimately give a lot of preference to A. This might result in getting the less common class B wrongly classified.

6. **Outlier sensitivity:** K-NN algorithm is very sensitive to outliers as it simply chose the neighbors based on distance criteria.

7. **Missing Value treatment:** K-NN inherently has no capability of dealing with missing value problem.

## Logistic Regression

- In logistic regression, the model produces a numeric estimate. However, the values of target variable in the data are categorical 
- Logistic regression is estimating the probability of class membership(a numeric quantity) over a categorical class
- Given data on time spent studying and exam scores. Linear Regression and logistic regression can predict different things:
  - **Linear Regression** could help us predict the student’s test score on a scale of 0 - 100. Linear regression predictions are continuous (numbers in a range).
  - **Logistic Regression** could help use predict whether the student passed or failed. Logistic regression predictions are discrete (only specific values or categories are allowed). **We can also view probability scores underlying the model’s classifications.** 

<img src='https://miro.medium.com/proxy/1*YNZEHUdHspzkOpCsPu_E9g.jpeg' style="zoom:50%;" >



![](https://miro.medium.com/max/1142/0*tGVPGu3aa1rhTdfl.png)

**Types of Logistic Regression:**

1. **Binary Logistic Regression**: The categorical response has only two 2 possible outcomes. E.g.: Spam or Not

2. **Multinomial Logistic Regression:** Three or more categories without ordering. E.g.: Predicting which food is preferred more (Veg, Non-Veg, Vegan)

3. **Ordinal Logistic Regression:** Three or more categories with ordering. E.g.: Movie rating from 1 to 5







Sigmoid Function:

$S(z) = \frac{1} {1 + e^{-z}}$





### Decision boundary

**Logistic Regression Equation:**

$$\boxed{\phi=p(y=1|x;\theta)=\frac{1}{1+\exp(-\theta^Tx)}=g(\theta^Tx)}$$

The underlying algorithm of Maximum Likelihood Estimation (MLE) determines the regression coefficient for the model that accurately predicts the probability of the binary dependent variable. The algorithm stops when the convergence criterion is met or maximum number of iterations are reached. Since the probability of any event lies between 0 and 1 (or 0% to 100%), when we plot the probability of dependent variable by independent factors, it will demonstrate an ‘S’ shape curve.

g(E(y)) = α + βx1 + γx2

Here, g() is the link function, E(y) is the expectation of target variable and α + βx1 + γx2 is the linear predictor (α,β,γ to be predicted). The role of link function is to ‘link’ the expectation of y to linear predictor.

Key Points :

1. GLM does not assume a linear relationship between dependent and independent variables. However, it assumes a **linear relationship between link function and independent variables in logit model.**
2. The dependent variable need not to be normally distributed.
3. It does not uses OLS (Ordinary Least Square) for parameter estimation. Instead, it uses maximum likelihood estimation (MLE).
4. Errors need to be independent but not normally distributed.





Our current prediction function returns a probability score between 0 and 1. In order to map this to a discrete class (true/false, cat/dog), we select a threshold value or tipping point above which we will classify values into class 1 and below which we classify values into class 2.

𝑝≥0.5,𝑐𝑙𝑎𝑠𝑠=1

𝑝<0.5,𝑐𝑙𝑎𝑠𝑠=0

Note: Cutoff level may vary!

For example, if our threshold was .5 and our prediction function returned .7, we would classify this observation as positive. If our prediction was .2 we would classify the observation as negative. For logistic regression with multiple classes we could select the class with the highest predicted probability.

![](https://ml-cheatsheet.readthedocs.io/en/latest/_images/logistic_regression_sigmoid_w_threshold.png)





## SoftMax Regression

Binary Classification -> Multiclass Classification 



**Ordered Logit** 

• Multiclass Classification -> Ordinal Categorical target





