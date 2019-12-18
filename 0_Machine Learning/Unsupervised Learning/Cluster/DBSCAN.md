**Density-based spatial clustering of applications with noise (DBSCAN)** 

### Parameters:

The DBSCAN algorithm basically requires 2 parameters:

**eps**: specifies how close points should be to each other to be considered a part of a cluster. It means that if the **distance** between two points is lower or equal to this value (eps), these points are considered neighbors.

**minPoints**: the minimum number of points to form a dense region. For example, if we set the minPoints parameter as 5, then we need at least 5 points to form a dense region.

![image.png](https://i.loli.net/2019/11/27/rpeA6EuBfDRVMKv.png)

### Parameter estimation:

**eps**: if the eps value chosen is too small, a large part of the data will not be clustered. It will be considered outliers because don’t satisfy the number of points to create a dense region. On the other hand, if the value that was chosen is too high, clusters will merge and the majority of objects will be in the same cluster. The eps should be chosen based on the distance of the dataset (we can use a k-distance graph to find it), but in general small eps values are preferable.

**minPoints**: As a general rule, a minimum minPoints can be derived from a number of dimensions (D) in the data set, as minPoints ≥ D + 1. Larger values are usually better for data sets with noise and will form more significant clusters. The minimum value for the minPoints must be 3, but the larger the data set, the larger the minPoints value that should be chosen.

### Density Reachability

![image.png](https://i.loli.net/2019/11/27/RiaLjdDxF8PQ9nc.png)





![image.png](https://i.loli.net/2019/11/27/lFPCciaOuwKjX1q.png)



### When DBSCAN Works Well

- Resistant to Noise 
- Can handle clusters of different shapes and sizes



### When DBSCAN Does Not Work Well

- Sensitive to parameters 
- Cannot handle varying densities

