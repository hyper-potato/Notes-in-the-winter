[psyyz10.github.io](http://psyyz10.github.io/2015/11/SDA/ "Stacked Denoising Autoencoders | YAO's BLOG")

# Stacked Denoising Autoencoders | YAO's BLOG

##  Autoencoders

An autoencoder [[Bengio09]][1] is a network whose graphical structure is shown in Figure [4.1][2], which has the same dimension for both input and output. It takes an unlabeled training examples in set <img src="https://i.loli.net/2019/12/06/LyUPW24vQXuKcg8.png" alt="image.png" style="zoom:40%;" /> where <img src="https://i.loli.net/2019/12/06/s6TGYIdRHwZB4yW.png" alt="image.png" style="zoom:40%;" />is a single input and encodes it to the hidden layer <img src="https://i.loli.net/2019/12/06/ahDSjPfU1OpumM9.png" alt="image.png" style="zoom:50%;" />by linear combination with weight matrix $W$ and then through a non-linear activation function. It can be mathematically expressed as <img src="https://i.loli.net/2019/12/06/bs4PY9iErTnOAeq.png" alt="image.png" style="zoom:50%;" />, where $b$ is the bias vector.

<img src="https://i.loli.net/2019/12/06/lzIs4TfXiQPEAmD.png" alt="image.png" style="zoom:50%;" />

Figure 4.1: An Autoencoder

 

After that the hidden layer representation will be reconstructed to the output layer $\hat{x}$ through a decoding function, in which 􏰌 $\hat{x}$  has a same shape as $x$. Hence the decoding function can be mathematically expressed as 􏰌<img src="https://i.loli.net/2019/12/06/KigsW6rJTDGS9Fj.png" alt="image.png" style="zoom:50%;" />, where <img src="https://i.loli.net/2019/12/06/VGRDqvw1P49phfr.png" alt="image.png" style="zoom:50%;" />can be <img src="https://i.loli.net/2019/12/06/IrlRoSiCTJuBXyM.png" alt="image.png" style="zoom:50%;" /> called tried weights. In this project, tied weights were used. The aim of the model is to optimize the weight matrices, so that the reconstruction error between input and output can be minimized. It can be seen that the Autoencoder can be viewed as an unsupervised learning process of encoding-decoding: the encoder encodes the input through multi-layer encoder and then the decoder will decode it back with the lowest error [[Hinton06]][15]. 
To measure the reconstruction error, traditional squared error <img src="https://i.loli.net/2019/12/06/zPtdE8pqDHrI12C.png" alt="image.png" style="zoom:50%;" /> can be used. One of the most widely used way to measure that is the cross entropy if the input can be represented as bit vector or bit possibilities. The cross entropy error is shown in Equation [4.1][17]:


<img src="https://i.loli.net/2019/12/06/MaLWhTYbHiZwXdR.png" alt="image.png" style="zoom:67%;" />

Equation:4.1

 


The hidden layer code $h$ can capture the information of input examples along the main dimensions of variant coordinates via minimizing the reconstruction error. It is similar to the principle component analysis (PCA) which project data on the main component that captures the main information of the data. h can be viewed as a compression of input data with some lost, which hopefully not contain much information about the data. It is optimized to compress well the training data and have a small reconstruction error for the test data, but not for the data randomly chosen from input space.

##  Denoising Autoencoders

In order to prevent the Autoencoder from just learning the identity of the input and make the learnt representation more robust, it is better to reconstruct a corrupted version of the input. The Autoencoder with a corrupted version of input is called a Denoising Autoencoder. Its structure is shown in Figure [4.2][20]. This method was proposed in [[Vincent08]][21], and it showed an advantage of corrupting the input by comparative experiments. Hence we will use denoising autoencoders instead of classic autoencoders in this thesis.


![][22]

Figure 4.2: A graphical figure of Denoising Autoencoder. An input x is corrupted to ![][23]. After that the autoencoder maps it to the hidden representation ![][19] and attempts to reconstruct ![][11].

 


A Denoising Autoencoder can be seen as a stochastic version with adding a stochastic corruption process to Autoencoder. For the raw inputs ![][10], some of them will be randomly set to 0 as corrupted inputs ![][23]. Next the corrupted input ![][23] will be en- coded to the hidden code and then reconstructed to the ouput. But now 􏰌![][10] is a deterministic function of ![][23] rather than ![][11]. As Autoencoder, the reconstruction is considered and calculated between 􏰌![][23] and ![][11] noted as ![][24]. The parameters of the model are initialized randomly and then optimized by stochastic gradient descent algorithms. The difference is that the input of the encoding process is a corrupted version ![][23], hence it forces a much more clever mapping than just the identity, which can denoise and extract useful features in a noise condition.

The training algorithm of a denoising autoencoder is summarized in Algorithm 2.

![](https://imgur.com/8CMkWQw)



##  Stacked Autoencoder

Unsupervised pre-training 
A Stacked Autoencoder is a multi-layer neural network which consists of Autoencoders in each layer. Each layer's input is from previous layer's output. The greedy layer wise pre-training is an unsupervised approach that trains only one layer each time. Every layer is trained as a denoising autoencoder via minimising the cross entropy in reconstruction. Once the first ![][26] layer has been trained, it can train the ![][27] layer by using the previous layer's hidden representation as input. An example is shown below. Figure [4.3][28] shows the first step of a stacked autoencoder. It trains an autoencoder on raw input ![][11] to learn ![][29] by minimizing the reconstruction error ![][24].


![][30]

Figure 4.3: Step 1 in Stacked Autoencoders

 


Next step shown in Figure [4.4][31]. The hidden representation ![][29] would be as "raw input" to train another autoencoder by minimizing the reconstruction error ![][32]. Note that the error is calculated between previous latent feature representation ![][33] and the output ![][34]. Parameters ![][35] and ![][36] will be optimized by the gradient descent algorithm. The new hidden representation h2 will be the 'raw input' of the next layer.


![][37]

Figure 4.4: Step 2 in Stacked Autoencoders

 

The pre-training algorithm of stacked denoising autoencoder is summarized in algorithm 3. 

![][38] 


##  Supervised fine-tuning

At last once all the layers has been pre-trained, the next step called fine-tuning is performed. A supervised predictor will be extended to the last layer, such as support vector machine or a logistic regression layer. In this project, we chose a logistic regression layer. After that the network will be trained. A sample graph is shown in Figure [4.5][39]. It can be seen that for each layer of the network, only the encoding hidden representation ![][40] are considered. The fine-tuning step will train the whole network by back-propagation like training an Artificial Neural Network. A stacked denoising autoencoder is just replace each layer's autoencoder with denoising autoencoder whilst keeping other things the same.


![][41]

Figure 4.5: A complete architecture of stacked autoencoder

 


The supervised fine-tuning algorithm of stacked denoising auto-encoder is summa- rized in Algorithm 4.

![][42] 
![][43] 





Deep Learning Overview

[1]: http://psyyz10.github.io#Bengio09
[2]: http://psyyz10.github.io#F4_1
[3]: http://psyyz10.github.io/img/1.png
[4]: http://psyyz10.github.io/img/2.png
[5]: http://psyyz10.github.io/img/3.png
[6]: http://psyyz10.github.io/img/4.png
[7]: http://psyyz10.github.io/img/5.png
[8]: http://psyyz10.github.io/img/6.png
[9]: http://psyyz10.github.io/img/7.png
[10]: http://psyyz10.github.io/img/8.png
[11]: http://psyyz10.github.io/img/9.png
[12]: http://psyyz10.github.io/img/10.png
[13]: http://psyyz10.github.io/img/11.png
[14]: http://psyyz10.github.io/img/36.png
[15]: http://psyyz10.github.io#Hinton06
[16]: http://psyyz10.github.io/img/12.png
[17]: http://psyyz10.github.io#E4_1
[18]: http://psyyz10.github.io/img/13.png
[19]: http://psyyz10.github.io/img/14.png
[20]: http://psyyz10.github.io#F4_2
[21]: http://psyyz10.github.io#Vincent08
[22]: http://psyyz10.github.io/img/15.png
[23]: http://psyyz10.github.io/img/16.png
[24]: http://psyyz10.github.io/img/18.png
[25]: http://psyyz10.github.io/img/19.png
[26]: http://psyyz10.github.io/img/20.png
[27]: http://psyyz10.github.io/img/21.png
[28]: http://psyyz10.github.io#E4_3
[29]: http://psyyz10.github.io/img/22.png
[30]: http://psyyz10.github.io/img/34.png
[31]: http://psyyz10.github.io#F4_4
[32]: http://psyyz10.github.io/img/25.png
[33]: http://psyyz10.github.io/img/24.png
[34]: http://psyyz10.github.io/img/26.png
[35]: http://psyyz10.github.io/img/27.png
[36]: http://psyyz10.github.io/img/28.png
[37]: http://psyyz10.github.io/img/29.png
[38]: http://psyyz10.github.io/img/30.png
[39]: http://psyyz10.github.io#F4_5
[40]: http://psyyz10.github.io/img/31.png
[41]: http://psyyz10.github.io/img/32.png
[42]: http://psyyz10.github.io/img/33.png
[43]: http://psyyz10.github.io/img/35.png
[44]: http://psyyz10.github.io/2015/11/SDA/#disqus_thread
[45]: http://psyyz10.github.io/tags/Deep-Learning/
[46]: http://psyyz10.github.io/tags/Machine-Learning/
[47]: http://psyyz10.github.io/2015/11/Deep-Learning-Overview/

