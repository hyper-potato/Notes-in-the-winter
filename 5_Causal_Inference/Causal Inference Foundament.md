# Causal Inference

Prediction and causation are very different. 

Typical questions are:

- Prediction: Predict Y after observing X = x
- Causation: Predict Y after setting X = x

Causation involves predicting the effect of an intervention. For example:

- Prediction: Predict health given that a person takes vitamin C 
- Causation: Predict health if I give a person vitamin C

The difference between passively observing X = x and actively intervening and setting X = x is significant and requires different techniques and, typically, much stronger assumptions. This is the area known as causal inference.

## Preliminaries
### Two Types of Causal Questions
There are two types of causal questions. 
1. The first deals with questions like this: do cell phones cause brain cancer? 
In this case, there are variables X and Y and we want to know the causal effect of X on Y . The challenges are: find a parameter θ that characterizes the causal influence of X on Y and find a way to estimate θ. This is usually what we mean when we refer to causal inference.

2. The second question is: given a set of variables, determine the causal relationship between the variables. This is called causal discovery. **This problem is statistically impossible** despite the large number of papers on the topic.


### Two Types of Data
Data can be from a controlled, randomized experiment or from an observational study.
In the former, X is randomly assigned to subjects. In the latter, it is not randomly assigned. In randomized experiments, causal inference is straightforward. In observational (non-randomized) studies, the problem is much harder and requires stronger assumptions and also requires subject matter knowledge. Statistics and Machine Learning cannot solve causal problems without background knowledge.

### Example
Consider this story. A mother notices that tall kids have a higher reading level than short kids. The mother puts her small child on a device and stretches the child until he is tall.
She is dismayed to find out that his reading level has not changed.
The mother is correct that height and reading skill are associated. Put another way, you
can use height to predict reading skill. But that does not imply that height causes reading skill. This is what statisticians mean when they say:
correlation is not causation.
On the other hand, consider smoking and lung cancer. We know that smoking and lung
cancer are associated. But we also believe that smoking causes lung cancer. In this case, we recognize that intervening and forcing someone to smoke does change his probability of getting lung cancer.

### Prediction Versus Causation
The difference between prediction (association/correlation) and causation is this: in prediction we are interested in
P(Y ∈ A|X = x)
which means: the probability that Y ∈ A given that **we observe that X is equal to x**. 
For causation we are interested in
P(Y ∈ A|set X = x)
which means: the probability that Y ∈ A given that **we set X equal to x**. 

Prediction is about passive observation. Causation is about active intervention. The phrase correlation is not causation can be written mathematically as P(Y ∈ A|X = x) != P(Y ∈ A|set X = x).

Causal inference is tricky and should be used with great caution. The main messages
are:
1. Causal effects can be estimated consistently from randomized experiments.
2. It is difficult to estimate causal effects from observational (non-randomized) experiments.
3. All causal conclusions from observational studies should be regarded as very tentative.

##  Counterfactuals
Consider two variables X and Y . We will call X the “exposure” or the “treatment.” We call Y the “response” or the “outcome.” For a given subject we see (Xi, Yi). What we don’t see is what their value of Yi would have been if we changed their value of Xi. This is called the counterfactual. The whole causal story is made clear in Figure 1 which shows data (left) and the counterfactuals (right).

![image.png](https://i.loli.net/2020/02/05/rkZ5GWiPcdXC1zm.png)

- Some take a philosophical view that potential or counterfactual outcomes may be thought to “exist” (“many-worlds” interpretation of quantum mechanics in physics). 
- ...but it could be argued that they are best interpreted as useful
tools (mental constructs) corresponding to what-if types of
structures in language.
- Only one of the outcomes is realized and observed (observable), given by 
Yi = Yi(0)(1 − Zi) + Yi(1)Zi

## Caution of R-square
Why $R^2$ is a bad metric for linear regression 
BMI ~ weight + height (wrong model)   R2 = 0.9
ln(BMI) ~ ln(weight) + ln(height)  (right model) R2 = 0.88

why?

1) It's not out of sample evaluation! We should withhold part of data to evaluation a model

2) R square is methmetically flaw. We 
   $R^{2} =  \frac{\sum (\hat{y} – \bar{\hat{y}})^{2}}{\sum (y – \bar{y})^{2}}$
   - R-squared does not measure goodness of fit. It can be arbitrarily low when the model is completely correct. By making  σ2  large, we drive R-squared towards 0, even when every assumption of the simple linear regression model is correct in every particular.
    
    What is  σ2 ? When we perform linear regression, we assume our model almost predicts our dependent variable. The difference between “almost” and “exact” is assumed to be a draw from a Normal distribution with mean 0 and some variance we call  σ2 .

    - R-squared can be arbitrarily close to 1 when the model is totally wrong.


Let’s recap:

* R-squared does not measure goodness of fit.
* R-squared does not measure predictive error.
* R-squared does not allow you to compare models using transformed responses.
* R-squared does not measure how one variable explains another.



https://data.library.virginia.edu/is-r-squared-useless/
https://www.reddit.com/r/statistics/comments/3ow1cd/my_stats_professor_just_went_on_a_rant_about_how/