[reference](https://medium.com/jameslearningnote/%E8%B3%87%E6%96%99%E5%88%86%E6%9E%90-%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-%E7%AC%AC5-1%E8%AC%9B-%E5%8D%B7%E7%A9%8D%E7%A5%9E%E7%B6%93%E7%B6%B2%E7%B5%A1%E4%BB%8B%E7%B4%B9-convolutional-neural-network-4f8249d65d4f)

<img src='https://miro.medium.com/max/3116/1*irWQaiIjHS27ZAPaVDoj6w.png'>

圖片經過各兩次的Convolution, Pooling, Fully Connected就是CNN的架構了，因此只要搞懂**Convolution, Pooling, Fully Connected**三個部分的內容就可以完全掌握了CNN



<img src='https://miro.medium.com/max/1120/1*RIBWK55dcDJa-zI_dFPDnw.png'>

## Convolution Layer卷積層

卷積運算就是將原始圖片的與特定的Feature Detector(filter)做卷積運算(符號⊗)，卷積運算就是將下圖兩個3x3的矩陣作相乘後再相加，以下圖為例 00 + 00 + 01+ 01 + 10 + 00 + 00 + 01 + 01 =0

![img](https://miro.medium.com/max/1856/1*CO0yrGvAE7jw6JfGqCMRPg.png)



依序做完整張表

![img](https://miro.medium.com/max/1838/1*Klv6ebMkjVmAEP4XkMxTXQ.png)

**卷積運算**

中間的Feature Detector(Filter)會隨機產生多种(ex:16種)，Feature Detector的目的就是幫助我們萃取出圖片當中的一些特征(ex:形狀)，就像人的大腦在判斷這個圖片是什麼東西也是根據形狀來推測

![](https://miro.medium.com/max/1822/1*AJeWQ88UnmfkJ4_sFOT-YA.png)



利用Feature Detector萃取出物體的邊界

![img](https://miro.medium.com/max/4116/1*MlGDfnY5W0yjA2iHj8K4vg.png)

**使用Relu函數去掉負值，更能淬煉出物體的形狀**

![](https://miro.medium.com/max/3656/1*BZqw3CWuvtc7sNVjgcBPQg.png)

## Pooling Layer 池化層

在Pooling Layer這邊主要是採用Max Pooling，Max Pooling的概念很簡單只要挑出矩陣當中的最大值，Max Pooling主要的好處是**當圖片整個平移幾個Pixel的話對判斷上完全不會造成影響，以及有很好的抗雜訊功能。**

![img](https://miro.medium.com/max/4604/1*-Yo6iC0S3QLWqgAPnV9knQ.png)



![img](https://miro.medium.com/max/4504/1*CGwpxQT5kJho3CbDZy2Qkw.png)

## Fully Connected Layer 全連接層

基本上全連接層的部分就是將之前的結果平坦化之後接到最基本的神經網絡了

![img](https://miro.medium.com/max/5244/1*7Q0lMIi6W-H5p_KuymEezw.png)

![img](https://miro.medium.com/max/4196/1*DPXziFXhXTw_xpfIdKFkqg.png)

## 卷积层尺寸的计算原理

- **输入矩阵**格式：四个维度，依次为：样本数、图像高度、图像宽度、图像通道数

- **输出矩阵**格式：与输出矩阵的维度顺序和含义相同，但是后三个维度（图像高度、图像宽度、图像通道数）的尺寸发生变化。

- **权重矩阵**（卷积核）格式：同样是四个维度，但维度的含义与上面两者都不同，为：卷积核高度、卷积核宽度、输入通道数、输出通道数（卷积核个数）

- **输入矩阵、权重矩阵、输出矩阵这三者之间的相互决定关系**

- - 卷积核的输入通道数（in depth）由输入矩阵的通道数所决定。（红色标注）
  - 输出矩阵的通道数（out depth）由卷积核的输出通道数所决定。（绿色标注）
  - 输出矩阵的高度和宽度（height, width）这两个维度的尺寸由输入矩阵、卷积核、扫描方式所共同决定。计算公式如下。（蓝色标注）

![[公式]](https://www.zhihu.com/equation?tex=+%5Cbegin%7Bcases%7D+height_%7Bout%7D+%26%3D+%28height_%7Bin%7D+-+height_%7Bkernel%7D+%2B+2+%2A+padding%29+~+%2F+~+stride+%2B+1%5C%5C%5B2ex%5D+width_%7Bout%7D+%26%3D+%28width_%7Bin%7D+-+width_%7Bkernel%7D+%2B+2+%2A+padding%29+~+%2F+~+stride+%2B+1+%5Cend%7Bcases%7D)



> \* 注：以下计算演示均省略掉了 Bias ，严格来说其实每个卷积核都还有一个 Bias 参数。

## 标准卷积计算举例

> 以 AlexNet 模型的第一个卷积层为例，
> \- 输入图片的尺寸统一为 227 x 227 x 3 （高度 x 宽度 x 颜色通道数），
> \- 本层一共具有96个卷积核，
> \- 每个卷积核的尺寸都是 11 x 11 x 3。
> \- 已知 stride = 4， padding = 0，
> \- 假设 batch_size = 256，
> \- 则输出矩阵的高度/宽度为 (227 - 11) / 4 + 1 = 55

![[公式]](https://www.zhihu.com/equation?tex=+%5Cbegin%7Bmatrix%7D+%26+%5Cmathbf%7BBatch%7D+%26+%5Cmathbf%7BHeight%7D+%26%26+%5Cmathbf%7BWidth%7D+%26%26+%5Cmathbf%7BIn~Depth%7D+%26%26+%5Cmathbf%7BOut~Depth%7D%5C%5C%5B2ex%5D+%5Cmathbf%7BInput%7D+%26+%5Cquad%5Cquad+256+%5Cquad%5Cquad+%5Ctimes+%26+%5Ccolor%7Bblue%7D%7B227%7D+%26+%5Ctimes+%26+%5Ccolor%7Bblue%7D%7B227%7D+%26+%5Ctimes+%26+%5Ccolor%7Bred%7D%7B3%7D+%5C%5C%5B2ex%5D+%5Cmathbf%7BKernel%7D+%26%5Cquad%5Cquad%5Cquad%5Cquad%5Cquad+%26+%5Ccolor%7Bblue%7D%7B11%7D+%26+%5Ctimes+%26+%5Ccolor%7Bblue%7D%7B11%7D+%26+%5Ctimes+%26+%5Ccolor%7Bred%7D%7B3%7D+%26+%5Ctimes+%26+%5Ccolor%7Bgreen%7D%7B96%7D+%5C%5C%5B2ex%5D+%5Cmathbf%7BOutput%7D+%26+%5Cquad%5Cquad+256+%5Cquad%5Cquad+%5Ctimes+%26+%5Ccolor%7Bblue%7D%7B55%7D+%26+%5Ctimes+%26+%5Ccolor%7Bblue%7D%7B55%7D+%26%26%26+%5Ctimes+%26+%5Ccolor%7Bgreen%7D%7B96%7D+%5Cend%7Bmatrix%7D)