# Weekly Self Assessment Questions

##  Week 2

1. How to select K in KNN? Why need a separate validation set?
1) try some K

2) Small values for K can be noisy and subject to the effects of outliers.

3) Larger values of K will have smoother decision boundaries which mean lower variance but increased bias.

4) cross-validation Plot different K vs accuracy for both training data and test data

5) In general, practice, choosing the value of **k** is **k = sqrt(N)** where **N** stands for the **number of samples in your training dataset**.

6) Try and keep the value of k odd in order to avoid confusion between two classes of data



2. Why we need to do feature normalization in KNN? Should normalization be done before or after the split of training and testing data?

Most of times different features in the data might be have varying magnitudes. KNN uses euclidean distance between data point in their computation. Having two features with different range of numbers will let the feature with bigger range dominate the algorithm.

Before spliting



3. We can use linear regression as classifier, why need logistic?

In classification, if we use Y as the outcome variable and tried to fit a line, it wouldn’t be a very good representation of the relationship. 

For linear regression, both *X* and *Y* ranges from minus infinity to positive infinity. 

Linear regression usually refers to continuity i.e. predicting continuous variables (medicine price, taxi fare etc.) depending upon features. However, **logistic regression is about predicting binary variables i.e when the target variable is categorical.** 

Since logistic regression is about classification, i.e *Y* is a categorical variable. It’s clearly not possible to achieve such output with linear regression model, since the range on both sides do not match.

You can see a relationship there–higher values of X are associated with more 0s and lower values of X have more 1s. But it’s not a linear relationship.



4. The difference of decision boundary between KNN and logistic regression.

KNN: Euclidean distance 

Logistic: probability cutoff

The underlying algorithm of Maximum Likelihood Estimation (MLE) determines the regression coefficient for the model that accurately predicts the probability of the binary dependent variable. The algorithm stops when the convergence criterion is met or maximum number of iterations are reached.



5. The difference of binary classification and multi-class classification.







The difference of ordered logit and multi-class classification.

Why need the Naive assumption in the Naive Bayesian classifier?

Could be very computationally costly, if the dataset has many attributes 

**Naïve assumption**: class conditional independence: 

*P*(*X*|*Ci*) = *P*(*x*1|*Ci*) *P*(*x*2|*Ci*) ... *P*(*xn*|*Ci*) 

- I.e., attributes are conditionally independent of each other, given class label 
- The assumption may not be very realistic is some domains 
- Still provides excellent prediction in many cases 

(*P*(*xk*|*Ci*) can be estimated from data)



8. How to calculate P(X_k|C_i) when X_k is categorical/numerical?

- when X_k is categorical: P(X_k|C_i)= |D_ik| / |D_i|, where |Dik| is the number of Ci data points in D that have x_k value of attribute Ak

- when X_k is numerical: Ak is typically assumed to have a normal (Gaussian) distribution, and therefore, is estimated as

  - P(X_k|C_i) 

  <img src='https://www.geeksforgeeks.org/wp-content/ql-cache/quicklatex.com-7fb78d7323fcbade0cb664161a8e84c4_l3.svg'>

  ​	(Mu_Ci, sigma - mean and standard deviation for Ak within Ci)

  - Alternative approach: discretize the attribute



9. What is the signal of your model is over-fitting and why almost all predictive models are prone to over-fitting?

The tendency of models to tailor models to the training data, at the expense of generalization to previously unseen data points





10. How to avoid over-fitting in decision tree, KNN and logistic?

    Tree-pruning

    - Pre-pruning: halting the tree induction early
      - threshold on the number of data points in each node
      - on total number of tree nodes
      - on attributes metrix
      - do statistical hypothesis test to evaluate whether information gain is due to chance
    - Post-pruning
      - performed after building the entire tree
      - caclulate expected error rates with and without a node(eg. using an internal training/validation data split), choose better



11. Why Naive Bayesian is less likely to suffered from over-fitting?

The Naive Bayes classifier employs a very simple (linear) hypothesis function. As a result it suffers from high bias, or error resulting from inaccuracies in its hypothesis class, because its hypothesis function is so simple it cannot accurately represent many complex situations. 

On the other hand, it exhibits low variance, or failure to generalize to unseen data based on its training set, because it's hypothesis class' simplicity prevents it from overfitting to its training data.



## Week 3

1. Why we need to do cross validation? The advantage of cross validation comparing with training/testing split.

- Different **modeling** procedures may have different performance on the same data 
- Different **training** sets may result in different generalization performance 
- Different **test sets** may result in different estimates of the generation performance 
- If the **training set size** changes, you may also expect different **generalization performance** from the resultant model 


1. How to implement cross validation by yourself based on the stratified sampling?

keep the same ratio of different classes in training dataset; make sure all data being included, result consistency

3. What are the two flaws of using training/validation/testing split to set the hyper parameter and evaluate the performance? How grid search via cross validation and nested cross validation fix these two flaws?

- if the model has many hyperparameters, we need a very large validation set, which is not always the case; only use little to calidate hyper-parameters of one model
- only use  little to choose model



4. What is the difference of purpose in grid search via cross validation and nested cross validation? Can we use nested cross validation to set the hyper-parameter directly? 

- grid search via cross validation: Train a model and tune (optimize) its hyperparameters

- grid search via nested cross validation: Build different models and compare different algorithms (e.g., SVM vs. logistic regression vs. Random Forests, etc.).

  - In nested cross-validation, we have an outer k-fold cross-validation loop to split the data into training and test folds, and an inner loop is used to select the model via k-fold cross-validation on the training fold. After model selection, the test fold is then used to evaluate the model performance. After we have identified our “favorite” algorithm, we can follow-up with a “regular” k-fold cross-validation approach (on the complete training set) to find its “optimal” hyperparameters and evaluate it on the independent test set.

  

5. How to implement nested cross validation?

- Create Inner Cross Validation (For Parameter Tuning)

This is our inner cross validation. We will use this to hunt for the best parameters for `C`, the penalty for misclassifying a data point. `GridSearchCV` will conduct steps 1-6 listed at the top of this tutorial.

```python
# Create a list of 10 candidate values for the C parameter
C_candidates = dict(C=np.logspace(-4, 4, 10))

# Create a gridsearch object with the support vector classifier and the C value candidates
clf = GridSearchCV(estimator=SVC(), param_grid=C_candidates)
```

The code below isn’t necessary for parameter tuning using nested cross validation, however to demonstrate that our inner cross validation grid search can find the best value for the parameter `C`, we will run it once here:

```python
# Fit the cross validated grid search on the data 
clf.fit(X_std, y)

# Show the best value for C
clf.best_estimator_.C
2.7825594022071258
```

- Create Outer Cross Validation (For Model Evaluation)

With our inner cross validation constructed, we can use `cross_val_score` to evaluate the model with a second (outer) cross validation.

The code below splits the data into three folds, running the inner cross validation on two of the folds (merged together) and then evaluating the model on the third fold. This is repeated three times so that every fold is used for testing once.

```python
cross_val_score(clf, X_std, y)
array([ 0.94736842,  0.97894737,  0.98412698])
```

Each the values above is an unbiased evaluation of the model’s accuracy, once for each of the three test folds. Averaged together, they would represent the average accuracy of the model found in the inner cross validated grid search.

<img src='https://mlfromscratch.com/content/images/size/w2000/2019/05/nested_cv.png'>









6. When accuracy/precision/recall measurement is not enough? Any alternative measurement can solve this problem?

- unbalanced class distribution

- unequal costs and benefits for different classes



7. We already have accuracy/precision/recall measurement, why still need ROC curve?

**ROC curves are independent of the class proportions as well as the costs and benefits**

**but not the most intuitive visulaization for many business stakeholders**



8. How to generating ROC curve? How the best/worst/random ROC curve likes like?



9. How to compare models based on ROC curve?



10. What is the inherent meaning of AUC?

ROC is a probability curve and **AUC represents degree or measure of separability**. It tells how much model is capable of distinguishing between classes. 



11. Why ROC curve is class proportion insensitive? Any solution to fix this problem? 

ROC curves plot FPR vs. TPR Because TPR only depends on positives, ROC curves do not measure the effects of negatives. The area under the ROC curve (AUC) assesses overall classification performance. AUC does not place more emphasis on one class over the other, so it does not reflect the minority class well

Precision-Recall (PR) curves will be more informative than ROC when dealing with highly skewed datasets. The PR curves plot precision vs. recall (FPR). Because Precision is directly influenced by class imbalance so the Precision-recall curves are better to highlight differences between models for highly imbalanced data sets. When you compare different models with imbalanced settings, the area under the Precision-Recall curve will be more sensitive than the area under the ROC curve.



12. How the best/worst/random precision-recall curve likes like? Why precision-recall curve is sensitive to unbalance data?

The PR curves plot precision vs. recall (FPR). 

Because **Precision is directly influenced by class imbalance** so the Precision-recall curves are better to highlight differences between models for highly imbalanced data sets. When you compare different models with imbalanced settings, the area under the Precision-Recall curve will be more sensitive than the area under the ROC curve.

12. How to generate lift curve in classification setting?
13. What is the usage of validation curve?

- The process of taking cost-aware matrix into prediction and how to do cost minimization prediction?

  

## Week 4

1. In the general data mining framework, what is the difference of a classification task with a regression task?

- numeric

- Categorical 

  

2. What is the difference of MAE/RMSE with MAPE?

MAE: mean absolute error 

![image.png](https://i.loli.net/2019/10/18/EQqdeTon7zCKpGM.png)

3. What is the difference between MAE and RMSE?

1. How to generate lift chart in regression task? The difference with lift chart in classification task.
2. The difference of error-based measurement and lift chart?
3. For KNN regression, what is the difference and similarity with KNN classification?
4. For regression tree, what is the difference and similarity with classification Tree?
5. How to do linear regression and any required pre-processing steps?
6. If the data is not linear curve, how can we use ploy-regression to fit a none-linear curve?
7. The difference of L1 and L2 regularization, why L1 is used for feature selection?
8. How to avoid overfitting for KNN regression, regression tree and linear/ploy regression?

## Week 5

- What is the build motivation of SVM?
- What is the optimization objective of SVM and what are the constrains of this optimization problem?
- Why a_i parameter is the indicator of support vector?
- Why we need to solve dual problem?
- How can we use the results from the dual problem to help us build the kernal function? What is kernal trick?
- When data is not linearly separable, how can we make adjustment of SVM?
- The usage of different kernels?
- The different motivation of SVM regression comparing with SVM regression? How this motivation leads to the different optimization problem?
- How to deal with not linearly regression case for SVM regression?
- How to avoid over-fitting in SVM classification and regression?







## Week 6

1. In predictive analytical domain, what is the source of error making? What is bias variance trade-off? 
2. The workflow of bagging framework. What problem bagging can solve?


3. If we have a bagging version of decision tree, what is the difference with random forest?
2. The workflow of boosting framework. What problem boosting can solve?
3. The difference of adaboosting/xgboosting/gradient boosting?
4. The workflow of stacking framework. What problem stacking can solve?
5. The different algorithm of feature selection, their difference and similarity.

##  Week 8-10

- The workflow of basic Neural Network

- How to adjust Neural Network architecture to solve difference problems? (e.g. regression/binary classification/multi-class classification)
- What happened if we just add more layers to the basic Neural Network architecture? Why it doesn't work out?
- How deep neural network solve the gradient vanish problem? Any Tricks?

The simplest solution is to use other activation functions, such as ReLU, which doesn’t cause a small derivative.

Residual networks are another solution, as they provide residual connections straight to earlier layers. As seen in Image 2, the residual connection directly adds the value at the beginning of the block, **x**, to the end of the block (F(x)+x). This residual connection doesn’t go through activation functions that “squashes” the derivatives, resulting in a higher overall derivative of the block.





- Why need drop out in deep neural network ?
- How to do regularization in deep neural network ? 
- Why batch normalization matters deep neural network ?
- What is the problem of Relu? How to choose activation function?
- The workflow of CNN.
- What does filter in CNN mean? Do we need to set/choose filter?
- Why we need Pooling layer in CNN?
- The workflow of LSTM and GRU. Why LSTM/GRU can combat the vanish gradient problem?

## Week 12-13

- The difference between collaborative-based and content-based recommendation approaches.
- The workflow of item-based/user-based collaborative filtering and their difference.
- What is clod start problem and how to solve this problem?
- How to evaluate the recommendation performance?
- Link deep neural network with recommender system