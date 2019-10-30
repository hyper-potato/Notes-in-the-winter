# Measurement Metrics



## Classification metrics



### Confusion matrix

In a context of a binary classification, here are the main metrics that are important to track in order to assess the performance of the model.



Confusion matrix ― The confusion matrix is used to have a more complete picture when assessing the performance of a model. 

|                    | **Predicted** class +               | -                                    |
| ------------------ | ----------------------------------- | ------------------------------------ |
| **Actual** class + | **TP** True Positives               | **FN** False Negatives Type II error |
| -                  | **FP** False Positives Type I error | **TN** True Negatives                |



### Main metrics

― The following metrics are commonly used to assess the performance of **classification** models:

| **Metric**                     | **Formula**                                                  | **Interpretation**                                           |
| ------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Accuracy                       | $\displaystyle\frac{\textrm{TP}+\textrm{TN}}{\textrm{TP}+\textrm{TN}+\textrm{FP}+\textrm{FN}}$ | Overall performance of model                                 |
| Precision                      | $\displaystyle\frac{\textrm{TP}}{\textrm{TP}+\textrm{FP}}$   | How accurate the positive predictions are; how many of the samples predicted by the classifier as positive is indeed positive. |
| Recall<br/>Sensitivity<br/>TPR | $\displaystyle\frac{\textrm{TP}}{\textrm{TP}+\textrm{FN}}$   | Coverage of actual positive sample; how many of the positive samples have been identified as being positive. |
| Specificity                    | $\displaystyle\frac{\textrm{TN}}{\textrm{TN}+\textrm{FP}}$   | Coverage of actual negative sample; how many of the negative samples have been identified as being negative. |
| F-measure                      | $\displaystyle2 * \frac{precision * recall}{precision + recall}$ |                                                              |
| F1 score                       | $\displaystyle\frac{2\textrm{TP}}{2\textrm{TP}+\textrm{FP}+\textrm{FN}}$ | Hybrid metric useful for unbalanced classes                  |
| FPR<br/>1-Specificity          | $\displaystyle\frac{\textrm{FP}}{\textrm{TN}+\textrm{FP}}$   |                                                              |



### ROC vs AUC

**ROC** ― The receiver operating curve, also noted ROC, is the plot of **TPR versus FPR** **(Recall vs 1-Specificity)** by varying the threshold. 

- Each point in a ROC curve arises from the values in the confusion matrix **associated with the application of a specific cutoff** on the predictions (scores) of the classifier.
- **ROC is a probability curve **
- **AUC represents degree or measure of separability.** It tells how much model is capable of distinguishing between classes. 
  - Higher the AUC, better the model is at predicting 0s as 0s and 1s as 1s. By analogy, Higher the AUC, better the model is at distinguishing between patients with disease and no disease.
- AUC is equivalent to the Mann-Whitney- Wilcoxon measure (MWW test)
- *Among all “positive”-“negative” pairs in the dataset compute the proportion of those which are ranked correctly by the evaluated classification algorithm.*

The ROC curve is plotted with TPR against the FPR where TPR is on y-axis and FPR is on the x-axis.

$\begin{align*} TPR &= \frac{TP}{TP + FN} \\ FPR &= \frac{FP}{FP + TN} \\ \end{align*}$



<img src='https://stanford.edu/~shervine/images/roc-auc.png'>



#### Interpretation



<img src='https://classeval.files.wordpress.com/2015/06/a-roc-curve-of-a-random-classifier.png?w=560&h=546'>

1. Ideal situation 

   <table><tr> <td> <img src='https://miro.medium.com/max/1056/1*Uu-t4pOotRQFoyrfqEvIEg.png' alt="Drawing" style="width: 250px;"/> </td><td> <img src='https://miro.medium.com/max/730/1*HmVIhSKznoW8tFsCLeQjRw.png' alt="Drawing" style="width: 250px;"/> </td></tr></table>
When two curves don’t overlap at all means model has an ideal measure of separability. It is perfectly able to distinguish between positive class and negative class.
   

   
2. A good one

   <table><tr> <td> <img src='https://miro.medium.com/max/1014/1*yF8hvKR9eNfqqej2JnVKzg.png' alt="Drawing" style="width: 250px;"/> </td><td> <img src='https://miro.medium.com/max/730/1*-tPXUvvNIZDbqXP0qqYNuQ.png' alt="Drawing" style="width: 250px;"/> </td></tr></table>

When two distributions overlap, we introduce type 1 and type 2 error. 

Depending upon the threshold, we can minimize or maximize them. When AUC is 0.7, it means there is 70% chance that model will be able to distinguish between positive class and negative class.



3. Bad situation

<table><tr> <td> <img src='https://miro.medium.com/max/860/1*iLW_BrJZRI0UZSflfMrmZQ.png' alt="Drawing" style="width: 250px;"/> </td><td> <img src='https://miro.medium.com/max/730/1*k_MPO2Q9bLNH9k4Wlk6v_g.png' alt="Drawing" style="width: 300px;"/> </td></tr></table>
This is the worst situation. When AUC is approximately 0.5, model has a discrimination equivilent to flipping a coin.

A bad classifier will output scores whose values are only slightly associated with the outcome. Such a classifier will **reach a high TPR only at the cost of a high FPR.**



#### ROC curves for multiple models



<img src='https://classeval.files.wordpress.com/2015/06/two-roc-curves.png?w=560&h=546'>

Two ROC curves represent the performance levels of two classifiers A and B. Classifier A clearly outperforms classifier B in this example.



#### 3 aspects that can be problematic with the ROC analysis

1. Use dissimilar datasets in one ROC plot

2. Rely only on AUC scores

   AUC scores are convenient to compare multiple classifiers. Nonetheless, it is also important to check the actual curves especially when evaluating the final model. Even when two ROC curves have the same AUC values, the actual curves can be quite different.

3. ROC analysis in conjunction with imbalanced datasets

   ROC becomes less powerful when used with imbalanced datasets. One effective approach to avoid the potential issues with imbalanced datasets is using the early retrieval area, which is a region with high specificity values in the ROC space. Checking this area is useful to analyse the performance with fewer false positives (or small false positive rate).



#### Relation between Sensitivity, Specificity, FPR and Threshold.

Increase classification threshold

- recall/sensiticity ⬆️ up or stay the same, since more negative values
- Precision probably raise, since reducing false position

When we decrease the threshold, we get more positive values thus it increases the sensitivity and decreasing the specificity.

As we know FPR is 1 - specificity. So when we increase TPR, FPR also increases and vice versa.



### Precision-recall curve

**Note: The ratio of positives and negatives defines the baseline.**

1. **A precision-recall curve of a random classifier**

A classifier with the random performance level shows a horizontal line as P / (P + N). This line separates the precision-recall space into two areas. The separated area above the line is the area of good performance levels. The other area below the line is the area of poor performance.

<img src='https://classeval.files.wordpress.com/2015/06/random-precision-recall-curve1.png?w=840&h=448' style="zoom:50%;" >



2. **A precision-recall curve of a perfect classifier**

A perfect classifier shows a combination of two straight lines. The end point depends on the ratio of positives and negatives. For instance, the end point is (1.0, 0.5) when the ratio of positives and negatives is 1:1, whereas (1.0, 0.25) when the ratio is 1:3.

<img src='https://classeval.files.wordpress.com/2015/06/perfect-precision-recall-curve1.png?w=840&h=448' style="zoom:50%;" >

3. **Precision-recall curves for multiple models**

<img src='https://classeval.files.wordpress.com/2015/06/two-precision-recall-curves.png?w=520&h=520' style="zoom:50%;" >

It is easy to compare several classifiers in the precision-recall plot. 

Curves **close to the perfect precision-recall curve** have a better performance level than the ones closes to the baseline. In other words, a curve above the other curve has a better performance level.



#### ROC curve vs precision-recall curve

The main difference between ROC curves and precision-recall curves is that the number of true-negative results is not used for making a PRC. 

|                  | x-axis        |                | y-axis      |                |
| ---------------- | ------------- | -------------- | ----------- | -------------- |
| Curve            | Concept       | Calculation    | Concept     | Calculation    |
| Precision-recall | Recall        | TP / (TP + FN) | Precision   | TP / (TP + FP) |
| ROC              | 1-specificity | FP / (FP + TN) | Sensitivity | TP / (TP + FN) |

<img src='https://classeval.files.wordpress.com/2015/06/roc-precision-recall-one-to-one-relationship.png?w=800&h=444'>

Remember, a ROC curve represents a relation between sensitivity (RECALL) and False Positive Rate (NOT PRECISION). Sensitivity is the other name for recall but the False Positive Rate is not PRECISION. 

Recall/Sensitivity is the measure of the probability that your estimate is 1 given all the samples whose true class label is 1. It is a measure of how many of the positive samples have been identified as being positive. 
Specificity is the measure of the probability that your estimate is 0 given all the samples whose true class label is 0. It is a measure of how many of the negative samples have been identified as being negative. 

PRECISION on the other hand is different. It is a measure of the probability that a sample is a true positive class given that your classifier said it is positive. It is a measure of how many of the samples predicted by the classifier as positive is indeed positive. 

**Note here that this changes when the base probability or prior probability of the positive class changes.** Which means **PRECISION depends on how rare is the positive class.** In other words, it is used when positive class is more interesting than the negative class. 

So, if your problem involves kind of searching a needle in the haystack when for ex: 

- the positive class samples are very rare compared to the negative classes, use a precision recall curve. 
- Othwerwise use a ROC curve because a ROC curve remains the same regardless of the baseline prior probability of your positive class (the important rare class). 



## Gain and Lift Charts

Gain or lift is a measure of the effectiveness of a classification model calculated as the ratio between the results obtained with and without the model. Gain and lift charts are visual aids for evaluating performance of classification models. However, in contrast to the confusion matrix that evaluates models on the whole population gain or lift chart evaluates model performance in a portion of the population. 

<img src='http://www.saedsayad.com/images/Gain_chart.png'>



<img src='http://www.saedsayad.com/images/Gain_chart_1.png'>



<img src='http://www.saedsayad.com/images/Chart_Gain_3.png'>

Lift Chart

The lift chart shows how much more likely we are to receive positive responses than if we contact a random sample of customers. For example, by contacting only 10% of customers based on the predictive model we will reach 3 times as many respondents, as if we use no model.

<img src='http://www.saedsayad.com/images/Chart_lift.png'>



**Limitations of Accuracy Measure** 

![image.png](https://i.loli.net/2019/10/18/k6XqNlYayv9cTiI.png)

![image.png](https://i.loli.net/2019/10/18/xvBodDeC3SyH59t.png)







## Correlation coefficient

### Matthews correlation coefficient

Matthews correlation coefficient (MCC) is a correlation coefficient calculated using all four values in the confusion matrix.

 

<img src='https://s0.wp.com/latex.php?latex=%5Cmathrm%7BMCC+%3D+%5Cdisplaystyle+%5Cfrac%7BTP+%5Ccdot+TN+-+FP+%5Ccdot+FN%7D%7B%5Csqrt%7B%28TP+%2B+FP%29%28TP+%2B+FN%29%28TN+%2B+FP%29%28TN+%2B+FN%29%7D%7D%7D&bg=ffffff&fg=333333&s=0&zoom=2' style="zoom:50%;" >



### Kappa Statistic



![image.png](https://i.loli.net/2019/10/18/RvU4gKBIskw7HjD.png)



















reference:

https://classeval.wordpress.com/introduction/introduction-to-the-precision-recall-plot/

https://www.datascienceblog.net/post/machine-learning/interpreting-roc-curves-auc/

https://www.datasciencecentral.com/profiles/blogs/understanding-and-interpreting-gain-and-lift-charts

