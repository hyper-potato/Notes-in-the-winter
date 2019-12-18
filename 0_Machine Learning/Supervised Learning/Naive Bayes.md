https://blog.floydhub.com/naive-bayes-for-machine-learning/

# Naive Bayes Classifier

The assumption made here is that the predictors/features are independent. That is presence of one particular feature does not affect the other. Hence it is called naive.

![](https://miro.medium.com/max/1020/1*tjcmj9cDQ-rHXAtxCu5bRQ.png)



Naive Bayes (NB) is ‘naive’ because **it makes the assumption that features of a measurement are independent of each other.** This is naive because it is (almost) never true. Here is why NB works anyway.

NB is a very intuitive classification algorithm. It asks the question, “*Given these features, does this measurement belong to class A or B?*”, and answers it by taking the proportion of all previous measurements with the same features belonging to class A multiplied by the proportion of all measurements in class A. If this number is bigger then the corresponding calculation for class B then we say the measurement belongs in class A. Simple, right?