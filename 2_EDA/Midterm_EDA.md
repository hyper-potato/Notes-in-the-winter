**MSBA 6410 Exam I Study Guide** 

The exam will be made up of 18-24 multiple choice/objective type questions and 5-7 short answer questions. It will be conducted on Canvas, but it will be a closed book/notes exam; we will use proctorio to help administer the exam and ensure there is no use of extra materials. The exam can include questions related to any information in the course: lectures, assigned readings, recitations, etc. In preparing for the exam focus your attention on mastering the following key topics that were covered in the class. Please note that the concepts under each topic is simply an overview, and concepts not specifically listed may also appear on the exam. 

 

## Data preparation for analysis

1. Tidy data

- Each variable in data set is placed in its own column
- Each observation is in its own row
- Each value is in its own cell

Key tidyr functions:

 ```R
gather() # Gather columns into key-value pairs 
spread() # Spread key-value pairs into columns 
separate() # Separate one column into multiple 
unite() # Unite multiple columns into one
 ```



2. Missing values

- MCAR: presence of missing data is unrelated to any other observed or unobserved variable 
- MAR: related to other values but unrelated to unobserved value
- NMAR: hard

Principled ways of dealing with missing data, based on the nature of how/why the data is missing

- Omitting rows with missing values or imputing missing values

- Other: The shadow matrix; nabular data 

**Finding and counting missing values** 

`is.na(df) `
`any(is.na(df)) `
`sum(is.na(df))`
`any_na(x)`
`n_miss(x)`
`prop_miss(x) `

```R
# Find rows with no missing values
complete.cases(df)
# Find missings with groupby
airquality %>% 
	group_by(Month) %>% 
	miss_var_summary()
```

 

3. Detecting **outliers** and dealing with them

A simple apprach is Boxplot:

![](https://miro.medium.com/max/18000/1*2c21SkzJMf3frPXPAR_gZA.png)



- Mild outliers (1 out of 150 in normally distributed data): 
  - < *Q*1 – 1.5**IQR**
  - \> Q3 + 1.5**IQR**
- Extreme outliers (1 out of 425,000 in n. d. data): 
  - < *Q*1 – 3**IQR**
  - \>Q3 + 3**IQR** 



**4. Summary statistics**
- The role of using summary statistics and visualization in getting your data ready for analysis 

- Importance of the basic data transformations and data-normalization 

  1) Transforming Data Values

  - Smoothing (for removing noise) 
  - Moving average, regression, etc. 
  - Discretization (Numeric to Categorical)
     o Binning, histogram analysis, clustering, concept hierarchies (e.g., time), ...
  - Aggregation 
    o For analysis at different granularities 

  - Categorical to Numeric 
    o If ordinal, equidistant categories, then map to equidistant interval points in [0, 1] range
    o Otherwise, create dummy variables for different categories (in predictive modeling tasks: m-1 dummies for m variables!!!) 



​		2) **Normalization** 
§ When do you use them and why 
 		o E.g., Min-Max or Z-score 
​		Log transformations: Normalization can be combined with other transformations (e.g., logarithmic transformations) for *highly skewed data* 



## Visualization

o  Understand the importance of visualization 

o  Recognize a bad visualization from a good one 

o  Understand the different types of purposes of visualization 

o  Have a good knowledge of *ggplot* and it’s various commands/options 

§ I may give you a command and ask you to choose what type of plot it might generate

 

## Communication

o  Understand the principles and ideas of communication that we have covered 

o  Be able to provide or critique narratives, interpretations of results, and recommendations. 



## Association rules

1. How are they used? 

2. What are the key parameters that govern their generation and evaluation

   

§   Support, confidence, lift 

3. Explain the Apriori algorithm. Why is it needed in the first place? How does it solve the computational challenges in generating the rules? 

find frequency item sets where support > min support setup

generate high confidence or lift rules from each frequent itemset 



## Clustering

Have a good understanding of the basic algorithms: k-means and hierarchical clustering 

§   How they work 

§   Strengths and limitations 


​	o Gaussian Mixture Models (Soft Clustering) 





2. Hierarchical Clustering

Advantages	 

• Hierarchical clustering output a hierarchy, ie a structure that is more informative  than  the unstructured set of flat clusters returned by k-means. Therefore, it is easier to decide  on the	number of clusters by looking at the dendrogram

• Easy to implement	



Disavantages	 

• It is not possible to undo the previous step: once the instances have been assigned to a cluster, they can no longer be moved around.	

• Time complexity: not suitable for large datasets	 

• Initial seeds have a strong impact on the final results;  The order of the data has an impact on the	final results 

• Very sensitive to outliers



o  How to judge the quality of a clustering solution? 

Measures of Clustering Performance 

• **Internal(intrinsic)**:How good the clustering is without any external information (e.g., **SSE**) 

- Sum-of-Squared-Errors(“cohesion”) 
- Silhouette coefficient(“cohesion”and “separation”) 
- A number of other complex mathematical approaches based on different definitions of cohesion and separation 

• **Relative**:Given two clustering solutions for the same data, which one is better 



o Determining the best number of clusters 

Elbow 



o  Have a good understanding (should be able to describe them in a job interview) it of the advanced algorithms: DBSCAN and GMM clustering 

§   Understand the link between k-means and GMM 

Dentisy based Clustering **DBSCAN**

- DBSCAN: Density-Based Spatial Clustering of Applications with Noise 

- Input parameters 

  o e: size (i.e., radius) of the “neighborhood” considered for every data point 

  o **MinPts**: density threshold (if larger than this many data points, the “neighborhood” would be considered dense) 

- Main concepts
  o e-neighborhood of point x, Ne(x)={yÎD|d(x,y)<= e} 

  o x is core object if |Ne(x)| >= MinPts 

  o y is density-reachable from x, if there exist a finite sequence core objects between y and x such that each of them belongs to e- neighborhood of the previous one 

  o x and y are density-connected if they are density-reachable from a common object 



#### K-mean vs DBSCAN

- In k-means clustering, each cluster is represented by a centroid, and points are assigned to whichever centroid they are closest to. In DBSCAN, there are no centroids, and clusters are formed by linking nearby points to one another.
- k-means requires specifying the number of clusters, ‘k’. DBSCAN does not require the input of K, but does require specifying two parameters which influence the decision of whether two nearby points should be linked into the same cluster. These two parameters are a distance threshold, εε (epsilon), and “MinPts” (minimum number of points), to be explained.
- k-means runs over many iterations to converge on a good set of clusters, and cluster assignments can change on each iteration. DBSCAN makes only a single pass through the data, and once a point has been assigned to a particular cluster, it never changes.



DBSCAN clustering can identify outliers, observations which won’t belong to any cluster. Since DBSCAN clustering identifies the number of clusters as well, it is very useful with unsupervised learning of the data when we don’t know how many clusters could be there in the data.

K-Means clustering may cluster loosely related observations together. Every observation becomes a part of some cluster eventually, even if the observations are scattered far away in the vector space. Since clusters depend on the mean value of cluster elements, each data point plays a role in forming the clusters. Slight change in data points *might* affect the clustering outcome. This problem is greatly reduced in DBSCAN due to the way clusters are formed.



#### link between k-means and GMM 

![](https://qph.fs.quoracdn.net/main-qimg-bcd9e0fe83ac3511caae5877a63480c8)

Now, the figure to the left shows some unclustered data. 

K-means/Mixture of Gaussians tries to break them into clusters.

Let's says we are aiming to break them into three clusters, as above. K means will start with the assumption that a given data point belongs to one cluster.Choose a data point. At a given point in the algorithm, we are **certain** that a point belongs to a red cluster. In the next iteration, we might revise that belief, and be **certain** that it belongs to the green cluster. However, remember, in each iteration, we are absolutely certain as to which cluster the point belongs to. This is the "hard assignment".

What if we are uncertain? What if we think, well, I can't be sure, but there is 70% chance it belongs to the red cluster, but also 10% chance its in green, 20% chance it might be blue. That's a **soft assignment**. The **Mixture of Gaussian model** helps us to **express this uncertainty**. It starts with some prior belief about how certain we are about each point's cluster assignments. As it goes on, it revises those beliefs. But it incorporates the degree of uncertainty we have about our assignment.



Under the hood, a Gaussian mixture model is very similar to *k*-means: it uses an expectation–maximization approach which qualitatively does the following:

1. Choose starting guesses for the location and shape
2. Repeat until converged:
   1. *E-step*: for each point, **find weights encoding the probability** of membership in each cluster
   2. *M-step*: for each cluster, update its location, normalization, and shape based on *all* data points, **making use of the weights**



The result of this is that each cluster is associated not with a hard-edged sphere, but with a smooth Gaussian model. Just as in the *k*-means expectation–maximization approach, this algorithm can sometimes miss the globally optimal solution, and thus in practice multiple random initializations are used.



o  Understand the general principle of expectation maximization and its applications 