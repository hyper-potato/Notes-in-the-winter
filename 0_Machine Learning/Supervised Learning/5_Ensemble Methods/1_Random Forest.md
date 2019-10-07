# Random Forest

Random Forest is an ensemble of Decision Trees, generally trained via the bagging method (or sometimes pasting).



One of the most famous and useful bagged algorithms is the **Random Forest**

The algorithm works as follows:

1. Sample m data sets D1,…,Dm from D with replacement.

2. For each Dj train a full decision tree hj() (max-depth = ∞) with one small modification: before each split randomly subsample **k ≤ d** features (without replacement) and only consider these for your split. (This further increases the variance of the trees.) Find the best from these k features. (for every single split, we sample k features a fresh)

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





