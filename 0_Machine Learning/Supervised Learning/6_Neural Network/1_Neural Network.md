# Neural Network

Neural network can do feature engineering automatically

why we cant use linear regression in each layer of neural networks?
it will ultimately become linear and no improvement at all.


why we use __Cross Entropy__?

Before neural network:
1) ReLU (Rectified Linear Unit)
2) Stochastic process
3) 

what is the benefit of deep learning over kernels?
- Deep learning scales linearly with more data, for example, we got one hundred thousand data points to a million data points, it just ten times slower; whereas, kernels matrixs are quadratic in the number of data points, so if we draw a figure with  the number of training data and accuacy we can get. At the begining every single data makes a huge difference, but as the number increases, the curve will slowly go asymptote（渐近）(deminishing). So in order to get kernel machines to work on more data, we have to do approximations, which cost us accuracy. Therefore, even data size is very big, the improvement is very little, the magnitude is roughly the same as much as what we have to pay for scaling the algorithm up.
![image.png](https://i.loli.net/2019/10/24/4qovGlV8ISgwzdi.png)

- Deep learning works really well in two domains where it's the killer, object recognition and images, and speech recognition


Two key in gradient descent:
- compute deriative

Batch: proportion of data
Epoch: 
(1)iteration：表示1次迭代（也叫training step），每次迭代更新1次网络结构的参数；
(2)batch-size：1次迭代所使用的样本量；
(3)Epoch：1个epoch表示过了1遍训练集中的所有样本。


Backpropogation:

Vanishing gradient problem:
- Sigmoid is not able to lead to gradient descent, no change moving forward.
- Use Relu to solve it: The amount of change in each layer can be 'passed on' to the next layer

- The last layer has larger gradients, learn very fast, already converge. 


Relu:
fast to compute
help us to solve gradient problem

But Relu is not perfect:
Many of neurals will die out(will not come back after certain rounds), so gradient of them will become zero, networks will become slim and shallow, cant learn 

ReLU - variant, to address the limitation of ReLU:
Leaky ReLU
Parametric ReLU 


Why do we need so many layers of ReLU, if they are all kinds of linear?
Universality: we can add so many lines, then it will become a curve that has various of shapes (one layer, it has just one turning point)
Why neural network works better?
- Modualization: divide a problem into steps (boys with long hair: boys or girls? ➡️ long or short?)
Very good for __unbalanced data__, to use the result from last layer!

use some basic information at lower layers, and reuse those information in the sequential layers to decide more useful features. get various combinations


SVM vs Deep learning:
- Kernel function will map data automatically into a high dimentional space for us, but it is still a hand-crafted result, we don't know what will happen before a kernel selection, we need to try.
- For Deep learning, it is a __learnable kernel__, it will map our data into a space that is linear separable


Keras is eaiser to develop, use it for standard network

Pytorch is powerful but difficult and complex



Question: Is it necessary to be fully connected?





** training is really slow for loss function, to solve it:
- SGD Stochastic Gradient Descent
- Mini-batch: makes training faster, 


Overfitting:
Early stop
regularization
dropout







# Neural Network


1.  设计一个神经网络时，输入层与输出层的节点数往往是固定的，中间层则可以自由指定；
2.  神经网络结构图中的拓扑与箭头代表着**预测**过程时数据的流向，跟**训练**时的数据流有一定的区别；
3.  结构图里的关键不是圆圈（代表“神经元”），而是连接线（代表“神经元”之间的连接）。每个连接线对应一个不同的**权重**（其值称为**权值**），这是需要训练得到的。



## Todo

Image shape

Feature shape

Transfer learning