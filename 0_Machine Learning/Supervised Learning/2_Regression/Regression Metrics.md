**Basic metrics** ― Given a regression model ff, the following metrics are commonly used to assess the performance of the model:

| Total sum of squares                                         | **Explained sum of squares**                                 | **Residual sum of squares**                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| $\displaystyle\textrm{SS}_{\textrm{tot}}=\sum_{i=1}^m(y_i-\overline{y})^2$ | $\displaystyle\textrm{SS}_{\textrm{reg}}=\sum_{i=1}^m(f(x_i)-\overline{y})^2$ | $\displaystyle\textrm{SS}_{\textrm{res}}=\sum_{i=1}^m(y_i-f(x_i))^2$ |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |

<img src='https://miro.medium.com/max/2162/1*8VM2PELQ-oeM0O3ya7BIyQ.png' style="zoom:50%;" >

## MSE

If instead of taking absolute values of residuals we’ll square them, we’ll get **Mean Squared Error(MSE)**:

$\text{MSE}(y, \hat{y}) = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$



## RMSE (Root Mean Square Error)

It represents the [sample standard deviation](https://en.wikipedia.org/wiki/Sample_standard_deviation) of the differences between predicted values and observed values (called residuals). Mathematically, it is calculated using this formula:

$\mbox{rmse} = \sqrt{\frac{\sum_{i=1}^{n}(y_i - \hat{y_i})^2}{n}}$



## MAE

$\mbox{MAE}= \frac{\sum_{i=1}^{n}|y_i - \hat{y_i}|}{n}$



MAE vs RMSE

What are in common:

- both MAE and RMSE can range from 0 to ∞.
- MAE and RMSE have the same units as the target values
- They are indifferent to the direction of the errors 
- The lower the metric value, the better

how are MAE and RMSE different?

- **RMSE** gives a relatively **high weight to large errors** due to the fact that the residual is squared before averaging
- On the other hand, averaging absolute values makes **MAE more robust to outliers**
- RMSE is differentiable (which is correct, but not really important for an evaluation metric)



### Mean Absolute Percentage Error (MAPE)

There is more than a single definition for percentage errors measures for any of these (RMSE → RMSPE or MAE → MAPE). Typically, MAPE is used to normalize – or weight – your errors by the inverse of the actual observation value, so I will go with this definition.



1. MEAN SQUARE ERROR (MSE)

   MSE has some advantages over MAE:
   1) MSE highlights large errors over small ones.
   2) MSE is differentiable which helps find minimum and maximum values using mathematical methods more effectively.

![image.png](https://i.loli.net/2019/10/18/EQqdeTon7zCKpGM.png)



## SMAPE

**Symmetric mean absolute percentage error**

What if ai=0，can we still use MAPE?  

<img src='https://wikimedia.org/api/rest_v1/media/math/render/svg/9d7003eba8a7ffe2379cd5c232adf78daa3d1edf'>





## Lift Chart for Numeric Prediction





**Coefficient of determination** ― The coefficient of determination, often noted R^2, provides a measure of how well the observed outcomes are replicated by the model

$\boxed{R^2=1-\frac{\textrm{SS}_\textrm{res}}{\textrm{SS}_\textrm{tot}}}$