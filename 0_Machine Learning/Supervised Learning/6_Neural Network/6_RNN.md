# RNN & LSTM

**1. LSTM only deal with gradient vanishing problem, not explosion **



**RNNs suffer from the problem of vanishing gradients, ** 

> Summing up, we have seen that RNNs suffer from vanishing gradients caused by long series of multiplications of small values, diminishing the gradients and causing the learning process to become degenerate.
>
> LSTMs solve the problem by creating a connection between the forget gate activations and the gradients computation, this connection creates a path for information flow through the forget gate for information the LSTM should not forget.





2. Parameter sharing 

**RNNs share parameters across different positions, so does LSTM**/ index of time/ time steps of the sequence, which makes it possible to generalize well to examples of different sequence length. RNN is usually a better alternative to position-independent classifiers and sequential models that treat each position differently.

How does a RNN share parameters? 

Each member of the output is produced using the same **update rule** applied to the previous outputs. Such update rule is often a (same) NN layer, as the “A” in the figure below .



![](https://miro.medium.com/max/4000/0*WdbXF_e8kZI1R5nQ.png)



**Parameters in the same LSTM for different length of time series are the same**(weights of RNN/LSTM networks shared across time)

There are pure theoretical reasons for parameter sharing:

- It helps in applying the model to examples of different lengths. While reading a sequence, if RNN model uses different parameters for each step during training, it won't generalize to unseen sequences of different lengths.
- Oftentimes, the sequences operate according to the same rules across the sequence. For instance, in NLP:

​                           "On Monday it was snowing"

​                           "It was snowing on Monday"

...these two sentences mean the same thing, though the details are in different parts of the sequence. Parameter sharing reflects the fact that we are performing the same task at each step, as a result, we don't have to relearn the rules at each point in the sentence.



3. note input shape in keras for LSTM:

time length; Scala or vector for each timestamp;