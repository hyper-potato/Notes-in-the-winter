## Tree-based and ensemble methods

These methods can be used for both regression and classification problems.

**CART** ― Classification and Regression Trees (CART), commonly known as decision trees, can be represented as binary trees. They have the advantage to be very interpretable.

**Random forest** ― It is a tree-based technique that uses a high number of decision trees built out of randomly selected sets of features. Contrary to the simple decision tree, it is highly uninterpretable but its generally good performance makes it a popular algorithm.

*Remark: random forests are a type of ensemble methods.*



**Boosting** ― The idea of boosting methods is to combine several weak learners to form a stronger one. The main ones are summed up in the table below:

| **Adaptive boosting**                                        | **Gradient boosting**                     |
| ------------------------------------------------------------ | ----------------------------------------- |
| - Known as Adaboost <br>- High weights are put on errors to improve at the next boosting step | Weak learners trained on remaining errors |

