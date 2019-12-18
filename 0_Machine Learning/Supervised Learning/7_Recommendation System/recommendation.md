# Recommendation System Note



![](https://miro.medium.com/max/690/1*G4h4fOX6bCJhdmbsXDL0PA.png)



## Content-based Systems ("Show me more of the same what I've liked")

This algorithm recommends products which are similar to the ones that a user has liked in the past. For example, if a person has liked the movie “Inception”, then this algorithm will recommend movies that fall under the same genre. But how does the algorithm understand which genre to pick and recommend movies from



Based on the cosine value, which ranges between -1 to 1, the movies are arranged in descending order and one of the two below approaches is used for recommendations:

- **Top-n approach**: where the top n movies are recommended (Here n can be decided by the business)
- **Rating scale approach**: Where a threshold is set and all the movies above that threshold are recommended





## Collaborative Filtering Systems ("Tell me what's popular among my peers")

If person A likes 3 movies, say Interstellar, Inception and Predestination, and person B likes Inception, Predestination and The Prestige, then they have almost similar interests. We can say with some certainty that A should like The Prestige and B should like Interstellar. The collaborative filtering algorithm uses “User Behavior” for recommending items. 

This is one of the most commonly used algorithms in the industry as it is not dependent on any additional information. 

- Memory-based methods are the most simplistic as they use no model whatsoever. They assume that predictions can be made on pure “memory” of past data and usually just employ a simple distance-measurement approach,  **like nearest neighbour.**

  - Input: Only a matrix of given user - item ratings

  - Output types;

    - A (numerical) prediction indicating to what degree the current user will like or dislike a certain item
    - A top-N list of recommended items

    

- Model-based approaches, on the other hand, always assume some kind of underlying model and basically try to make sure that whatever predictions come out will fit the model well.



### User-based Collaborative Filtering Systems (Memory-based) (UB-CF)

#### Similarity

**Pearson’s Correlation**: It tells us how much two items are correlated. Higher the correlation, more will be the similarity. Pearson’s correlation can be calculated using the following formula:

![](https://miro.medium.com/max/770/1*qCdw27XS0Q9shX4-0pJ96w.png)



#### Algorithm

Two tasks:

<img src="https://miro.medium.com/max/1528/1*VtQk9w-8E4N1aHnzQiAqqA.png" style="zoom:50%;" />

1. Find the K-nearest neighbors (KNN) to the user **a,** using a similarity function **w** to measure the distance between each pair of users:

![](https://i.imgur.com/FKd69V6.png)



2. Predict the rating that user **a** will give to all items the **k** neighbors have consumed but **a** has not. We Look for the item **j** with the best predicted rating.

![](https://i.imgur.com/VgRuebU.png)



In other words, we are creating a User-Item Matrix, predicting the ratings on items the active user has not see, based on the other similar users. This technique is **memory-based**.

#### Limitation

- More users than items, The computational cost for UBCF in the worst case is O(NM), $U^I$ because it requires examining N customers and up to M items for each customer;
- For a single user the data in a row is sparse, and for each time an user rates a couple of new items, his close neighbors will change which make the model wont be very robust and stable, also the matrix needs to be updated again and again

Others:

PROS:

- Easy to implement.
- Context independent.
- Compared to other techniques, such as content-based, it is more accurate.

CONS:

- Sparsity: The percentage of people who rate items is really low.
- Scalability: The more **K** neighbors we consider (under a certain threshold), the better my classification should be. Nevertheless, the more users there are in the system, the greater the cost of finding the nearest K neighbors will be.
- Cold-start: New users will have no to little information about them to be compared with other users.
- New item: Just like the last point, new items will lack of ratings to create a solid ranking (More of this on [‘How to sort and rank items’](https://medium.com/@cfpinela/recommender-systems-ranking-and-classifications-3abae71d6fbf)).



### Item-Based Collaborative Filtering (IB-CF)

We could divide IB-CF in two sub tasks:

1. Calculate similarity among the items:
   - Cosine-Based Similarity
   - Correlation-Based Similarity
   - Adjusted Cosine Similarity
   - 1-Jaccard distance

2. Calculation of Prediction:
   - Weighted Sum
   - Regression

The difference between UB-CF and IB-CF  is that, in this case, we directly **pre-calculate the similarity between the co-rated items,** skipping K-neighborhood search.

#### Algorithm

**Step 1: transpose the user-item matrix to the item-user matrix**

![](https://miro.medium.com/max/1164/1*fCLzef5jXq5KyHT3R9bTAA.png)

**Step 2: Calculate the similarity between any two items and fill up the item-item similarity matrix**

![](https://miro.medium.com/max/1558/1*7dxyTP5Yd6-Fheu4E4hlzg.png)

**Step 3: Predict the ratings of movies that rated by an user**

![](https://i.imgur.com/Cwava2G.png)Here,

- *Pu,i* is the prediction of an item
- *Rv,i* is the rating given by a user *v* to a movie *i*
- *Su,v* is the similarity between users



The computation will be $I^U$, ($U^I$ for user based)

In terms of the computational cost for IBCF, there are two parts — building the item-item similarity matrix and predicting the ratings. For building the item-item matrix, O(N²M) is required in the worst Case, and O(NM) is the computational cost in reality due to the sparsity in the user-item matrix. **In regard to the prediction, the computational cost only depends on the movies that the user has rated, which is usually very little.**





Takeaway:

- A small change in column wont dramatically change in neighbors
- If we have more user than item, we use item based filter; if item>user, user-to-user based is more suitable
- if we have a new user, the column will be empty, so we need to associate content information to deal with it







|               | Pros                                                         | Cons                                                         |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Collaborative | No knowledge-engineering effort, **serendipity** of results, learns market segments | Requires some form of rating feedback, **cold start** **for new users and new items** |
| Content-based | No community required, comparison between items possible     | Content descriptions necessary, cold **start for new users**, **no surprises** |

## Model Based

![](https://miro.medium.com/max/2696/1*0vyDJr3urOA6uy-39cr91g.png)



 **Brief explanation of above mentioned algorithms:**

- **Matrix Factorization (MF):** The idea behind such models is that attitudes or preferences of a user can be determined by a small number of hidden factors. We can call these factors as **Embeddings.**

***Matrix decomposition can be reformulated as an optimization problem with loss functions and constraints.\*** *Now the constraints are chosen based on property of our model. For e.g. for Non negative matrix decomposition, we want non negative elements in resultant matrices.*

![](https://miro.medium.com/max/3000/1*2i-GJO7JX0Yz6498jUvhEg.png)









## Hybrid Recommendations









