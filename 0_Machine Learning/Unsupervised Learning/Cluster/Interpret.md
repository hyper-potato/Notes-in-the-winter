Interpret Results and Adjust Clustering

![ClusteringFlowchart.svg](https://developers.google.com/machine-learning/clustering/images/ClusteringFlowchart.svg)

Step One: Quality of Clustering
not rigourous because of the lack of 'truth'

- Cluster cardinality:
Cluster cardinality is the number of examples per cluster. Plot the cluster cardinality for all clusters and investigate clusters that are major outliers. For example, in Figure 2, investigate cluster number 5.
![ClusterCardinality.png](https://developers.google.com/machine-learning/clustering/images/ClusterCardinality.png)


- Cluster magnitude
the sum of distances from all examples to the centroid of the cluster. Similar to cardinality, check how the magnitude varies across the clusters, and investigate anomalies. For example, in Figure 3, investigate cluster number 0.
![ClusterMagnitude.png](https://developers.google.com/machine-learning/clustering/images/ClusterMagnitude.png)

- Magnitude vs. Cardinality

Notice that a higher cluster cardinality tends to result in a higher cluster magnitude, which intuitively makes sense. Clusters are anomalous <!--/əˈnɒmələs/different from what you expected to find: a highly anomalous situation; anomalous results --> when cardinality doesn't correlate with magnitude relative to the other clusters. 
Find anomalous clusters by plotting magnitude against cardinality. For example, in Figure 4, fitting a line to the cluster metrics shows that cluster number 0 is anomalous.
![CardinalityVsMagnitude.png](https://developers.google.com/machine-learning/clustering/images/CardinalityVsMagnitude.png)

Performance of downstream system


Step Two: Performance of the Similarity Measure


Step Three: Optimum Number of Clusters
k-means requires you to decide the number of clusters k beforehand. 