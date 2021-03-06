# Random Forest

Random Forest is an ensemble of Decision Trees, generally trained via the bagging method (or sometimes pasting).

(random forest has a 'feature importance' by product.

## the differences between bagged trees and random forests?

Bagging has a single parameter, which is the number of trees. All trees are fully grown binary tree (unpruned) and at each node in the tree one searches over all features to find the feature that best splits the data at that node.

Random forests has 2 parameters:

1. The first parameter is the same as bagging (the number of trees)
2. The second parameter (unique to random forests) is mtry which is **how many features to search over to find the best feature.** this parameter is usually 1/3*D for regression and sqrt(D) for classification. thus during tree creation randomly mtry number of features are chosen from all available features and the best feature that splits the data is chosen.

![](https://qph.fs.quoracdn.net/main-qimg-3926c524e0ec7751912b74c59075c721)





One of the most famous and useful bagged algorithms is the **Random Forest**



## The algorithm works as follows:

1. Sample m data sets D1,…,Dm from D with replacement.

2. For each Dj, train a full decision tree hj() (max-depth = ∞) with one small modification: before each split randomly subsample **k ≤ d** features (without replacement) and only consider these for your split. (This further increases the variance/randomness of the trees.) Find the best from these k features. (for every single split, we sample k features a fresh)

   (Ensure Bias is roughly low)

3. The final classifier is $h(\mathbf{x})=\frac{1}{m}\sum_{j=1}^m h_j(\mathbf{x})$.

    

    

   **hyperparameters: K and m**   

   **No need to scale**

**The Random Forest is one of the best, most popular and easiest to use out-of-the-box classifier.**



There are two reasons for this:

- The RF only has two hyper-parameters, m and k. It is extremely *insensitive* to both of these. **A good choice for k is $k=\sqrt{d}$ **(where d denotes the number of features). You can set m as large as you can afford.
- Decision trees do not require a lot of preprocessing. For example, the features can be of different scale, magnitude, or slope. This can be highly advantageous in scenarios with heterogeneous data, for example the medical settings where features could be things like blood pressure, age, gender, ..., each of which is recorded in completely different units.



### Useful variants of Random Forests:

- Split each training set into two partitions $D_l=D_l^A\cup D_l^B$, where $D_l^A\cap D_l^B=\emptyset$. Build the tree on $D_l^A$ and estimate the leaf labels on $D_l^B$. You must stop splitting if a leaf has only a single point in $D_l^B$ in it. This has the advantage that each tree and also the RF classifier become [consistent](https://en.wikipedia.org/wiki/Consistency_(statistics)).
- Do not grow each tree to its full depth, instead prune based on the leave out samples. This can further improve your bias/variance trade-off.





