---
layout: "post"
title: "ROC curve"
date: "2019-10-05 14:40"
---

# ROC curve vs precision-recall curve

There is a very important difference between what a ROC curve represents vs that of a PRECISION vs RECALL curve.

Remember, a ROC curve represents a relation between sensitivity (RECALL) and False Positive Rate (NOT PRECISION). Sensitivity is the other name for recall but the False Positive Rate is not PRECISION.

Recall/Sensitivity is the measure of the probability that your estimate is 1 given all the samples whose true class label is 1. It is a measure of how many of the positive samples have been identified as being positive.

Specificity is the measure of the probability that your estimate is 0 given all the samples whose true class label is 0. It is a measure of how many of the negative samples have been identified as being negative.

PRECISION on the other hand is different. It is a measure of the probability that a sample is a true positive class given that your classifier said it is positive. It is a measure of how many of the samples predicted by the classifier as positive is indeed positive. Note here that this changes when the base probability or prior probability of the positive class changes. Which means PRECISION depends on how rare is the positive class. In other words, it is used when positive class is more interesting than the negative class.

So, if your problem involves kind of searching a needle in the haystack when for ex: the positive class samples are very rare compared to the negative classes, use a precision recall curve. Othwerwise use a ROC curve because a ROC curve remains the same regardless of the baseline prior probability of your positive class (the important rare class).

```python
import sklearn
```
