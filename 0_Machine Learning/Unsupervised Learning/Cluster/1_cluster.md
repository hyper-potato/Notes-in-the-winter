# Clustering

## Basics
**Grouping** unlabeled examples is called clustering.

As the examples are unlabeled, clustering relies on unsupervised machine learning.  If the examples are labeled, then clustering becomes classification.

- Desired properties of clustering result: 
  o High intra-similarity 
  ​	• I.e., any two data points / objects that are assigned into the same cluster should exhibit similarity to each other (e.g., customers from the same demographic segment) 

  o Low inter-similarity 
  ​	• I.e., any two data points / objects that are assigned into different clusters should not be very similar to each other, otherwise they should have been assigned into the same cluster (e.g., customers with very different demographic information) 


### Similarity

- **Direct similarity** (based on direct distance, one might assume that points B and C are more likely to be in the same cluster than points A and B) 
- **Contextual similarity** (with additional information, it is clear that points A and B intuitively belong to the same cluster, and that B and C should be in different clusters) 
- Conceptual similarity (in the general/philosophical sense, the concept of similarity can become very complex; e.g., here A and B belong to the same cluster not because of direct distance or the context of other points, but because they represent a common underlying concept, i.e., a rectangle, as opposed to C which represents a different concept, a triangle) 


### Metrics

- Two records: *A*=(*a*1, ..., *ak*), *B*=(*b*1, ..., *bk*)
   o Here *k* is the number of attributes in your dataset (e.g., table) 

- Euclidean distance 

  o Definition: d(*A*,*B*) = SquareRoot((*a*1-*b*1)^2+...+(*ak* - *bk*)^2) 

  o Most popular, intuitive, and widely used distance metric for numeric data 
  Because, when *k* = 2 or *k* = 3, it represents the standard real-world direct distance in 2- dimensional or 3-dimensional space 

- Manhattan distance 
  o Definition: d(*A*, *B*) = |*a*1-*b*1|+...+|*ak*-*bk*| 
  o Used where “rectilinear” distance is relevant
  E.g., driving distance in a city with a grid-like street layout 

- Max-coordinate (or Chebyshev) distance 

  o Definition: d(*A*,*B*) = max*i* |*a**i*-*b**i*|
   E.g., distance between two chessboard squares, as measured in the number of king’s moves needed to go from one square to the other

- If *k* = 1, the above three metrics become identical 

  o d(*A*,*B*) = |*a*1-*b*1| 


### Measures of Clustering Performance 

- **Internal(intrinsic)**:How good the clustering is without any external information (e.g., **SSE**) 
- Sum-of-Squared-Errors(“cohesion”) 
- Silhouette coefficient(“cohesion”and “separation”) 
- A number of other complex mathematical approaches based on different definitions of cohesion and separation 

- **Relative**:Given two clustering solutions for the same data, which one is better 

- Determining the best number of clusters 
Elbow 


### Numeric Distance Metrics: Normalizing

- **Scaling/normalizing** is used to standardize the intervals before measuring distances 

  o **min-max** **scaling** numerical attribute to interval [0,1] 

  **z = (x– min) / (max – min)** 

  - *x*–original value of the attribute 
  - min–smallest value of the attribute
  - max–largest value of the attribute
  - z*–resulting(scaled) value of the attribute in the range[0,1] 

  o **z-score scaling** to standardize the intervals
   **z =(x–m)/s                //z-score** 

  • m– mean value of the attribute 

  • s– standard deviation(or mean absolute deviation,which is more robust to outliers than st.dev.) 

- When attributes have different importance **Weighted distances** may be used
   o E.g., d(*A*,*B*) = *w*1|*a*1-*b*1|+...+*wk*|*ak*-*bk*| 



## Clustering Algorithms
### Types of Clustering

1. Partition Algorithms(Flat, Centroid-based Clustering):
  - K-means
  - Mixureof Gaussian
  - Spectral Clustering

<br>

2. Hierarchical Algorithms:

  - Bottom up (agglomerative) 
    - Most popular and widely used hierarchical technique 
    - Begins with n clusters (where each individual data record is its own cluster) 
    - Repeatedly keeps joining two most similar clusters at a time, until only one cluster is left (i.e., the entire data set) 
    - All the intermediate merges are recorded in a special kind of data structure (“**dendrogram**”), which represents the main result of the clustering technique 

    <img src="https://miro.medium.com/max/848/1*3pMZjFiiaaLcfSZBKDjbXA.png" style="zoom:50%;" />
    
    <img src='https://miro.medium.com/max/1638/1*JPQRbJDw2E1_HEvwzVTDDw.jpeg' style="zoom:50%;" >
    
    Dendrogram representation
    
    <img src="https://miro.medium.com/max/1638/1*fw1vlNtq2vPFmAXsBy1_dA.jpeg" style="zoom:50%;" />


  - Top down (divisive)

<br>


3. Density-based Clustering(DBSCAN)
Density-based clustering connects areas of high example density into clusters. This allows for arbitrary-shaped distributions as long as dense areas can be connected. These algorithms have difficulty with data of varying densities and high dimensions. Further, by design, these algorithms do not assign outliers to clusters.


<br>

4. Distribution-based Clustering (GMM)
This clustering approach assumes data is composed of distributions, such as Gaussian distributions. As distance from the distribution's center increases, the probability that a point belongs to the distribution decreases. The bands show that decrease in probability. When you do not know the type of distribution in your data, you should use a different algorithm.


## Hierarchical Clustering

### Advantages	 

• Hierarchical clustering output a hierarchy, ie a structure that is more informative  than  the unstructured set of flat clusters returned by k-means. Therefore, it is easier to decide  on the	number of clusters by looking at the dendrogram

• Easy to implement	

### Disavantages	 

• It is not possible to undo the previous step: once the instances have been assigned to a cluster, they can no longer be moved around.	
• Time complexity: not suitable for large datasets	 
• Initial seeds have a strong impact on the final results;  The order of the data has an impact on the	final results 
• Very sensitive to outliers


## K-Means

Advantages	 
• Easy to implement	 
• With a large number of variables, K-Means may be computationally faster than  hierarchical	clustering (if K is small).	 
• k-Means may produce tighter clusters than hierarchical clustering	 
• An instance can change cluster (move to another cluster) when the centroids are recomputed.

Potential **Limitation** of K-Means 
• Sensitive to outliers 
​	o When outliers are assigned to a cluster, can distort the cluster mean 
• Compatibility only with continuous data 
​	o Most real-world data will have categorical features 
• Unable to capture hierarchical structure 
• Specification of a distance metric 
​	o (Semi-)Supervised Clustering 
• Initial seeds have a strong impact on the final results;  The order of the data has an impact on the	final results
• Hard Clustering   





## Dentisy based Clustering **DBSCAN**

- DBSCAN: Density-Based Spatial Clustering of Applications with Noise 

- Input parameters 
  o e: size (i.e., radius) of the “neighborhood” considered for every data point 
  o **MinPts**: density threshold (if larger than this many data points, the “neighborhood” would be considered dense) 

- Main concepts
  o e-neighborhood of point x, Ne(x)={yÎD|d(x,y)<= e} 

  o x is core object if |Ne(x)| >= MinPts 

  o y is density-reachable from x, if there exist a finite sequence core objects between y and x such that each of them belongs to e- neighborhood of the previous one 

  o x and y are density-connected if they are density-reachable from a common object 



### K-mean vs DBSCAN

- In k-means clustering, each cluster is represented by a centroid, and points are assigned to whichever centroid they are closest to. In DBSCAN, there are no centroids, and clusters are formed by linking nearby points to one another.
- k-means requires specifying the number of clusters, ‘k’. DBSCAN does not require the input of K, but does require specifying two parameters which influence the decision of whether two nearby points should be linked into the same cluster. These two parameters are a distance threshold, εε (epsilon), and “MinPts” (minimum number of points), to be explained.
- k-means runs over many iterations to converge on a good set of clusters, and cluster assignments can change on each iteration. DBSCAN makes only a single pass through the data, and once a point has been assigned to a particular cluster, it never changes.

DBSCAN clustering can identify outliers, observations which won’t belong to any cluster. Since DBSCAN clustering identifies the number of clusters as well, it is very useful with unsupervised learning of the data when we don’t know how many clusters could be there in the data.

K-Means clustering may cluster loosely related observations together. Every observation becomes a part of some cluster eventually, even if the observations are scattered far away in the vector space. Since clusters depend on the mean value of cluster elements, each data point plays a role in forming the clusters. Slight change in data points *might* affect the clustering outcome. This problem is greatly reduced in DBSCAN due to the way clusters are formed.



## DMM

### link between k-means and GMM 

Under the hood, a Gaussian mixture model is very similar to *k*-means: it uses an expectation–maximization approach which qualitatively does the following:

1. Choose starting guesses for the location and shape
2. Repeat until converged:
   1. *E-step*: for each point, **find weights encoding the probability** of membership in each cluster
   2. *M-step*: for each cluster, update its location, normalization, and shape based on *all* data points, **making use of the weights**

The result of this is that each cluster is associated not with a hard-edged sphere, but with a smooth Gaussian model. Just as in the *k*-means expectation–maximization approach, this algorithm can sometimes miss the globally optimal solution, and thus in practice multiple random initializations are used.


![](https://qph.fs.quoracdn.net/main-qimg-bcd9e0fe83ac3511caae5877a63480c8)

The figure to the left shows some unclustered data. 

K-means/Mixture of Gaussians tries to break them into clusters.

K means will start with the assumption that a given data point belongs to one cluster. Choose a data point. At a given point in the algorithm, we are **certain** that a point belongs to a red cluster. In the next iteration, we might revise that belief, and be **certain** that it belongs to the green cluster. However, remember, in each iteration, we are absolutely certain as to which cluster the point belongs to. This is the "hard assignment".

What if we are uncertain? What if we think, well, I can't be sure, but there is 70% chance it belongs to the red cluster, but also 10% chance its in green, 20% chance it might be blue. That's a **soft assignment**. The **Mixture of Gaussian model** helps us to **express this uncertainty**. It starts with some prior belief about how certain we are about each point's cluster assignments. As it goes on, it revises those beliefs. But it incorporates the degree of uncertainty we have about our assignment.


