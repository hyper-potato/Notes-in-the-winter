# Create a Manual Similarity Measure

1. Convert data to numeric value
To calculate the similarity between two examples, you need to combine all the feature data for those two examples into a single numeric value.

For instance, shoe size.
quantify how similar two shoes are by calculating the difference between their sizes. The smaller the numerical difference between sizes, the greater the similarity between shoes. Such a handcrafted similarity measure is called a manual similarity measure.

What if you wanted to find similarities between shoes by using both size and color? Color is categorical data, and is harder to combine with the numerical size data. We will see that as data becomes more complex, creating a manual similarity measure becomes harder. When your data becomes complex enough, you won’t be able to create a manual measure. That’s when you switch to a supervised similarity measure, where a supervised machine learning model calculates the similarity.

Suppose the model has two features: shoe size and shoe price data. Since both features are numeric, you can combine them into a single number representing similarity as follows.

Size (s): Shoe size probably forms a Gaussian distribution. __Confirm this__. Then normalize data.
Price (p): The data is probably a Poisson distribution. __Confirm this__. If you have enough data, convert the data to quantiles and scale to.

Combine the data by using root mean squared error (RMSE). Here, the similarity is .

eg. color -- numeric RGB values
    postcode --  latitude and longitude
    





-- <cite>[Create a Manual Similarity Measure  \|  Clustering in Machine Learning](https://developers.google.com/machine-learning/clustering/similarity/manual-similarity)</cite>