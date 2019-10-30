Contraints of ensemble modeling

1. diverse models
2. each model is a good learner, not too lousy, good enough to make a rough direction



how bagging will ensure 1st constraint: Every single set just contiain 66% of original data, 

2nd: bagging does majority voting, average them will 





Trees/stumps:

- shallow trees /stumps: large bias but less variances, less vulnerble to change in training data 

-  deep trees: less bias, large bias-



Address:

- bagging keeps merit of less bias, and reduce variance 

- boosting keeps merit of less variance, and reduce bias



Similarity:

use the same kind of model



Difference:

- Bagging: before voting, all the candidtes can be trained ;Boosting: they must train sequencially 

- Bagging is using majority voting to decide, boosting we are summing up the weight

- Gradient boosting is a more generilized framework than adaboosting, because it has a error measurement, 







they are genral framework, can be actually fed into  any algorithms,but they are used in trees more  mainly because each learner should be easy  to learn





















![image.png](https://i.loli.net/2019/10/09/UVEvdL3JOzSMte1.png)

