# Gibbs Sampling



LDA works by first making a key assumption: **the way a document was generated was by picking a set of topics **and then **for each topic picking a set of words.** 

1. Assume there are *k* topics across all of the documents
2. Distribute these *k* topics across document *m* (this distribution is known as α and can be symmetric or asymmetric, more on this later) by assigning each word a topic.
3. For each word *w* in document *m*, assume its topic is wrong but every other word is assigned the correct topic.
4. Probabilistically assign word *w* a topic based on two things:
   \- what topics are in document *m
   -* how many times word *w* has been assigned a particular topic across all of the documents (this distribution is called *β*, more on this later)
5. Repeat this process a number of times for each document and you’re done!





To start with, let’s randomly assign weights to both the matrices and assume that our data is generated as per the following steps:

\1. Randomly choose a topic from the distribution of topics in a document based on their assigned weights. In the previous example, let’s say we chose pink topic

\2. Next, based on the distribution of words for the chosen topic, select a word at random and put it in the document

\3. Repeat this step for the entire document

In this process, if our guess of the weights is wrong, then the actual data that we observe will be very unlikely under our assumed weights and data generating process. For example, let’s say we have document D1 which consists of the following text:

“*Qualcomm® Adreno™ 630 Visual Processing Subsystem featuring room-scale 6DoF with SLAM, Adreno Foveation*”

and let’s say we assign high weights to topic T1 which has high weights for words like Spoon, Plates, Onions etc. then we will see that given our assumption of how data is generated, it is very unlikely that T1 belongs to D1 or these words belongs to T1. Therefore, what we are doing is we are trying to maximize the likelihood of our data given these two matrices.

To identify the correct weights, we will use an algorithm called Gibbs sampling. Let’s now understand what Gibbs sampling is and how does it work in LDA.



**Gibbs Sampling**

As I mentioned earlier, to start with, we will assume that we know ϴ and Ф matrices. Now what we will do is, we will slowly change these matrices and get to an answer that maximizes the likelihood of the data that we have. We will do this on word by word basis by changing the topic assignment of one word. We will assume that we don’t know the topic assignment of the given word but we know the assignment of all other words in the text and we will try to infer what topic will be assigned to this word.

If we look at this in mathematical manner, what we are doing is we are trying to find conditional probability distribution of a single word’s topic assignment conditioned on the rest of the topic assignments. Ignoring all the mathematical calculations, what we will get is a conditional probability equation that looks like this for a single word *w* in document *d* that belongs to topic *k*:

![img](https://miro.medium.com/max/2294/1*UOi6N82wvIAxLIPThFUK4A.jpeg)



where:

n(d,k): Number of times document d use topic k

v(k,w): Number of times topic k uses the given word

αk: Dirichlet parameter for document to topic distribution

λw: Dirichlet parameter for topic to word distribution

There are two parts two this equation. First part tells us how much each topic is present in a document and the second part tells how much each topic likes a word. Note that for each word, we will get a vector of probabilities that will explain how likely this word belongs to each of the topics. In the above equation, it can be seen that the Dirichlet parameters also acts as smoothing parameters when *n(d,k)* or *v(k,w)* is zero which means that there will still be some chance that the word will choose a topic going forward.

Let’s go through an example now:

To start with, suppose we have a document with some random word topic assignment, for example, as shown below:

![img](https://miro.medium.com/max/2622/1*Sd9UEq7na8XCAQHPlg1HiA.jpeg)

We also have our count matrix v(k,w) as shown below:

![img](https://miro.medium.com/max/2610/1*DoTtkQ6F3hO_LJwfj0Vtlg.jpeg)

Now let’s change the assignment of word **world** in the document.

- First, we will reduce the count of world in topic 1 from 28 to 27 as we don’t know to what topic world belongs.
- Second let’s represent the matrix *n(d,k)* in the following way to show how much a document use each topic

![img](https://miro.medium.com/max/2548/1*wsTlAs6N5zT2tlxJmhQ6qg.jpeg)

· Third, let’s represent *v(k,w)* in the following way to show how many times each topic is assigned to this word

![img](https://miro.medium.com/max/2542/1*qop7pkRye81ZSWq3gpqpxQ.jpeg)

- Fourth, we will multiply these two matrices to get our conditional probabilities

![img](https://miro.medium.com/max/2788/1*aq4DK1E0GNR7AwsmdrUSsw.jpeg)

- Finally, we will randomly pick any of the topic and will assign that topic to **world** and we will repeat these steps for all other words as well. Intuitively, topic with highest conditional probability should be selected but as we can see other topics also have some chance to get selected

That’s it. This what Gibbs sampling algorithm is doing under the hood. Although we skipped some details like hyperparameter tuning, but from an intuition perspective, this is how Gibbs sampling works for topic modeling.

So, this is it for the theory of Latent Dirichlet Allocation. I hope this was helpful in understanding what topic modelling is and how we use LDA with Gibbs for topic modelling. In the next article, I will post the implementation of LDA using Python.