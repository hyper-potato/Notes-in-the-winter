# Clustering

Grouping unlabeled examples is called clustering.

As the examples are unlabeled, clustering relies on unsupervised machine learning. 

If the examples are labeled, then clustering becomes classification.

## Clustering Algorithms

### Types of Clustering

- Centroid-based Clustering
Centroid-based clustering organizes the data into __non-hierarchical__ clusters, in contrast to hierarchical clustering defined below. __k-means__ is the most widely-used centroid-based clustering algorithm. Centroid-based algorithms are efficient but sensitive to initial conditions and outliers. This course focuses on k-means because it is an efficient, effective, and simple clustering algorithm.

Examples grouped into clusters using centroid-based clustering.
           The lines show borders between clusters.
<img src="https://developers.google.com/machine-learning/clustering/images/CentroidBasedClustering.svg" alt="7d71eaa7.png" style="zoom: 33%;" />
Figure 1: Example of centroid-based clustering.

- Density-based Clustering
Density-based clustering connects areas of high example density into clusters. This allows for arbitrary-shaped distributions as long as dense areas can be connected. These algorithms have difficulty with data of varying densities and high dimensions. Further, by design, these algorithms do not assign outliers to clusters.

Examples grouped into two clusters using density-based clustering. The clusters are not separable linearly.
<img src="https://developers.google.com/machine-learning/clustering/images/DensityClustering.svg" alt="874689f0.png" style="zoom:33%;" />
Figure 2: Example of density-based clustering.

- Distribution-based Clustering
This clustering approach assumes data is composed of distributions, such as Gaussian distributions. In Figure 3, the distribution-based algorithm clusters data into three Gaussian distributions. As distance from the distribution's center increases, the probability that a point belongs to the distribution decreases. The bands show that decrease in probability. When you do not know the type of distribution in your data, you should use a different algorithm.

Examples clustered using distribution-based clustering. The shading of the density of examples in each cluster shows how the clusters map to distributions.
<img src="https://developers.google.com/machine-learning/clustering/images/DistributionClustering.svg" alt="0feb1a62.png" style="zoom:33%;" />
Figure 3: Example of distribution-based clustering.

- Hierarchical Clustering
Hierarchical clustering creates a tree of clusters. Hierarchical clustering is well suited to hierarchical data, such as taxonomies. In addition, another advantage is that any number of clusters can be chosen by cutting the tree at the right level.

Animals clustered by using a hierarchical tree.
![02e9ea4c.png](https://developers.google.com/machine-learning/clustering/images/HierarchicalClustering.svg)
Figure 4: Example of a hierarchical tree clustering animals.