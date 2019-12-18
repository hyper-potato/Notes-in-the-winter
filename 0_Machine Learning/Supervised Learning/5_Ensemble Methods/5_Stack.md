In Stacking, training data is split into two sets. 

The first set is used for training each model (layer-1, weak learners), the second one is used for training of combiner of predictions (layer-2, 'blender').

![](https://miro.medium.com/max/2522/1*ZucZsXkOwrpY2XaPh6teRw@2x.png)

For example, for a classification problem, we can choose as weak learners a KNN classifier, a logistic regression and a SVM, and decide to learn a neural network as meta-model. Then, the neural network will take as inputs the outputs of our three weak learners and will learn to return final predictions based on it.

So, assume that we want to fit a stacking ensemble composed of L weak learners. Then we have to follow the steps thereafter:

- split the training data in two folds
- choose L weak learners and fit them to data of the first fold
- for each of the L weak learners, make predictions for observations in the second fold
- fit the meta-model on the second fold, using predictions made by the weak learners as inputs





**Why do we have to split training data to train two layers?**

You have to have real, out-of-sample, predictions as the input to your blender, otherwise your blender is not learning about, and thereby improving, prediction accuracy - but instead learning about, and thereby improving, in-sample estimation accuracy, which can lead to overfitting. This is why you cannot use the whole training set for both layers - if you do, some of the "predictions" made by the base models will actually be in-sample estimates, not out-of-sample predictions.

You split your training data set so that subset 1 is used to train your base models. Your base models then are used to generate predictions for subset 2, and these predictions, along with the actuals for subset 2, are given to your blender for training. This is what is shown in your right picture. Basically, the predictions are features that are given to your blender, along possibly with other features from subset 2.

The model that the blender comes up with based on subset 2 is then used to predict the test data. This can be done by predicting the test data with each of the base models (developed on subset 1), then predicting the test data again with the blender model (developed on subset 2) + the predictions from the base models. The resultant predictions are the ones you use for calibrating / testing the combined base models + blender model.



(The models are not outputting predictions for the training set, they are outputting estimates for the training set. **The difference is key: predictions are out-of-sample, estimates are in-sample. If your objective is to improve predictive power, you need to train the blender on predictions and actual values, not on in-sample estimates and actual values.** Just evaluating the blender on the test data set doesn't change the fact that it was fitted to in-sample estimates with the target data being the very same data used in the base model fitting)