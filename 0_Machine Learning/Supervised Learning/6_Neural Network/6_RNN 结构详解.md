http://codewithzhangyi.com/2018/10/31/NLP%E7%AC%94%E8%AE%B0-RNN/

NLP笔记 - RNN结构详解

NLP里最常用最传统的深度学习模型就是循环神经网络 RNN（Recurrent Neural Network）。这个模型的命名已经说明了数据处理方法，是按顺序按步骤读取的。与人类理解文字的道理差不多，看书都是一个字一个字，一句话一句话去理解的。

- RNN
- Encoder-Decoder（ = Seq-to-Seq ）
- Attention Mechanism


## RNN 多结构详解
### CNN vs RNN

CNN需要固定长度的输入、输出，RNN的输入和输出可以是不定长且不等长的
CNN只有 one-to-one 一种结构，而RNN 有多种结构，如下图：
![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/001.png?raw=true)

1. one-to-one
最基本的单层网络，输入是x，经过变换Wx+b和激活函数f得到输出y
<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/001.jpg?raw=true" style="zoom:50%;" />

2. one-to-n
输入不是序列而输出为序列的情况，只在序列开始进行输入计算：
<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/009.jpg?raw=true" style="zoom:50%;" />


图示中记号的含义是：
- 圆圈或方块表示的是向量。
- 一个箭头就表示对该向量做一次变换。如上图中和x分别有一个箭头连接，就表示对和x各做了一次变换。
还有一种结构是把输入信息X作为每个阶段的输入：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/010.jpg?raw=true" style="zoom:50%;" />

下图省略了一些X的圆圈，是一个等价表示：
<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/011.jpg?raw=true" style="zoom:50%;" />


这种 one-to-n 的结构可以处理的问题有：

- 从图像生成文字（image caption），此时输入的X就是图像的特征，而输出的y序列就是一段句子，就像看图说话等
- 从类别生成语音或音乐等

3. **n-to-n**
最经典的RNN结构，输入、输出都是等长的序列数据。
假设输入为每个x是一个单词的词向量。
<img src="https://pic3.zhimg.com/80/v2-0f8f8a8313867459d33e902fed97bd16_hd.jpg" style="zoom:50%;" />
如：

- 自然语言处理问题。x1可以看是第一个单词，x2可以看做是第二个单词，依次类推
- 语音处理。此时，x1、x2、x3...是每帧的声音信号
- 时间序列问题。例如每天的股票价格等等

为了建模序列问题，RNN引入了隐状态h（**hidden state**）的概念，h可以对序列形的数据提取特征，接着再转换为输出。
先从计算开始看：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/003.jpg?raw=true" style="zoom:50%;" />

要注意的是，**在计算时，每一步使用的参数U、W、b都是一样的，也就是说每个步骤的参数都是共享的**，这是RNN的重要特点，一定要牢记。

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/004.jpg?raw=true" style="zoom:50%;" />
依次计算剩下来的（使用相同的参数U、W、b）：
<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/005.jpg?raw=true" style="zoom:50%;" />

这里为了方便起见，只画出序列长度为4的情况，实际上，这个计算过程可以无限地持续下去。得到输出值的方法就是直接通过h进行计算：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/006.jpg?raw=true" style="zoom:67%;" />

正如之前所说，一个箭头就表示对对应的向量做一次类似于f(Wx+b)的变换，这里的这个箭头就表示对h1进行一次变换，得到输出y1。

剩下的输出类似进行（使用和y1同样的参数V和c）：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/007.jpg?raw=true" style="zoom:50%;" />

这就是最经典的RNN结构，我们像搭积木一样把它搭好了。它的输入是x1, x2, .....xn，输出为y1, y2, ...yn，也就是说，输入和输出序列必须要是等长的。
由于这个限制的存在，经典RNN的适用范围比较小，但也有一些问题适合用经典的RNN结构建模，如：

- 计算视频中每一帧的分类标签。因为要对每一帧进行计算，因此输入和输出序列等长。
- 输入为字符，输出为下一个字符的概率。这就是著名的Char RNN（详细介绍请参考：The Unreasonable Effectiveness of Recurrent Neural Networks，Char RNN可以用来生成文章，诗歌，甚至是代码，非常有意思)

4. n-to-one
要处理的问题输入是一个序列，输出是一个单独的值而不是序列，应该怎样建模呢？实际上，我们只在最后一个h上进行输出变换就可以了：
<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/008.jpg?raw=true" style="zoom:50%;" />

这种结构通常用来处理序列分类问题, 如输入一段文字判别它所属的类别，输入一个句子判断其情感倾向，输入一段视频并判断它的类别等等。





## Encoder-Decoder
### n-to-m
还有一种是 n-to-m，输入、输出为不等长的序列。

这种结构是**Encoder-Decoder**，也叫Seq2Seq，是RNN的一个重要变种。原始的n-to-n的RNN要求序列等长，然而我们遇到的大部分问题序列都是不等长的，如机器翻译中，源语言和目标语言的句子往往并没有相同的长度。**为此，Encoder-Decoder结构先将输入数据编码成一个上下文语义向量c：**

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/012.jpg?raw=true)



语义向量c可以有多种表达方式，最简单的方法就是把Encoder的最后一个隐状态赋值给c，还可以对最后的隐状态做一个变换得到c，也可以对所有的隐状态做变换。

**拿到c之后，就用另一个RNN网络对其进行解码**，这部分RNN网络被称为Decoder。Decoder的RNN可以与Encoder的一样，也可以不一样。具体做法就是将c当做之前的初始状态输入到Decoder中：

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/013.jpg?raw=true)

还有一种做法是将c当做每一步的输入：
![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/014.jpg?raw=true)

### Encoder-Decoder 应用
由于这种Encoder-Decoder结构不限制输入和输出的序列长度，因此应用的范围非常广泛，比如：

- 机器翻译：Encoder-Decoder的最经典应用，事实上这结构就是在机器翻译领域最先提出的。
- 文本摘要：输入是一段文本序列，输出是这段文本序列的摘要序列。
- 阅读理解：将输入的文章和问题分别编码，再对其进行解码得到问题的答案。
- 语音识别：输入是语音信号序列，输出是文字序列。
### Encoder-Decoder 框架
Encoder-Decoder 不是一个具体的模型，是一种框架。

- Encoder：将 input序列 →转成→ 固定长度的向量
- Decoder：将 固定长度的向量 →转成→ output序列
- Encoder 与 Decoder 可以彼此独立使用，实际上经常一起使用
因为最早出现的机器翻译领域，最早广泛使用的转码模型是RNN。其实模型可以是 CNN /RNN /BiRNN /LSTM /GRU /…

### Encoder-Decoder 缺点
- 最大的局限性：编码和解码之间的唯一联系是固定长度的语义向量c
- 编码要把整个序列的信息压缩进一个固定长度的语义向量c
- 语义向量c无法完全表达整个序列的信息
- 先输入的内容携带的信息，会被后输入的信息稀释掉，或者被覆盖掉
- 输入序列越长，这样的现象越严重，这样使得在Decoder解码时一开始就没有获得足够的输入序列信息，解码效果会打折扣
因此，为了弥补基础的 Encoder-Decoder 的局限性，提出了attention机制。

## Attention Mechanism
注意力机制（attention mechanism）是对基础Encoder-Decoder的改良。Attention机制通过在每个时间输入不同的c来解决问题，下图是带有Attention机制的Decoder：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/015.jpg?raw=true" style="zoom:50%;" />


每一个c会自动去选取与当前所要输出的y最合适的上下文信息。具体来说，我们用$a_{ij}$衡量Encoder中第j阶段的hj和解码时第i阶段的相关性，最终Decoder中第i阶段的输入的上下文信息$c_i$ 就来自于所有$h_i$ 对$a_{ij}$的加权和。

以机器翻译为例（将中文翻译成英文）：

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/016.jpg?raw=true)

输入的序列是“我爱中国”，因此，Encoder中的h1、h2、h3、h4就可以分别看做是“我”、“爱”、“中”、“国”所代表的信息。在翻译成英语时，第一个上下文c1应该和“我”这个字最相关，因此对应的 $a_{11}$就比较大，而相应的$a_{12}$,$a_{13}$,$a_{14}$就比较小。c2应该和“爱”最相关，因此对应的$a_{22}$就比较大。最后的c3和h3、h4最相关，因此$a_{33}$,$a_{34}$的值就比较大。

至此，关于Attention模型，只剩最后一个问题了，那就是：这些权重$a_{ij}$是怎么来的？

事实上，$a_{ij}$同样是从模型中学出的，它实际和Decoder的第i-1阶段的隐状态、Encoder第j个阶段的隐状态有关。

同样还是拿上面的机器翻译举例，$a_{1j}$的计算（此时箭头就表示对h’和 同时做变换）：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/017.jpg?raw=true" style="zoom:50%;" />

$a_{2j}$的计算：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/018.jpg?raw=true" style="zoom:50%;" />

$a_{3j}$的计算：

<img src="https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/019.jpg?raw=true" style="zoom:50%;" />

以上就是带有Attention的Encoder-Decoder模型计算的全过程。

### Attention 的优点：
- 在机器翻译时，让生词不只是关注全局的语义向量c，增加了“注意力范围”。表示接下来输出的词要重点关注输入序列种的哪些部分。根据关注的区域来产生下一个输出。
- 不要求编码器将所有信息全输入在一个固定长度的向量中。
- 将输入编码成一个向量的序列，解码时，每一步选择性的从序列中挑一个子集进行处理。
- 在每一个输出时，能够充分利用输入携带的信息，每个语义向量不一样，注意力焦点不一样。
### Attention 的缺点
- 需要为每个输入输出组合分别计算attention。50个单词的输出输出序列需要计算2500个attention。
- attention在决定专注于某个方面之前需要遍历一遍记忆再决定下一个输出是以什么。
- Attention的另一种替代方法是强化学习，来预测关注点的大概位置。但强化学习不能用反向传播算法端到端的训练。

### Attention可视化
Attention的可视化是一件非常cool的事！下面推荐一些优秀的attention相关论文。

paper: Attention Is All You Need
https://arxiv.org/pdf/1706.03762.pdf
下图是句子的单词之间的attention联系，颜色深浅表示大小。

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/002.png?raw=true)


![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/003.png?raw=true)
paper: Show, Attend and Tell
https://arxiv.org/pdf/1502.03044v1.pdf
注意每个句子的不同单词在图片中的attention标注：

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/2018/11/RNN_01/004.png?raw=true)



paper: Convolutional Sequence to Sequence Learning
https://arxiv.org/pdf/1705.03122.pdf
机器翻译中attention数值的可视化，一般是随着顺序强相关的。



其它paper：

[Grammar as a Foreign Language]

[Teaching Machines to Read and Comprehend]

下面这张Attention热力图表达出了，当人在阅读的时候，注意力大多数放在了文字标题或者段落第一句话，还是蛮符合现实情况的。



Attention Mechanism是很早的概念了，但随着2017年谷歌的一篇《Attention Is All You Need》被完全引爆。随后会写这篇论文的详细走读和过程推导😎，敬请期待~

参考

完全图解RNN、RNN变体、Seq2Seq、Attention机制
RNN梯度消失和爆炸的原因

打赏2块钱，帮我买杯咖啡，继续创作，谢谢大家！☕~
赏
# NLP # RNN
NLP实战 - 基于SimNet的Quora问句语义匹配
NLP笔记 - LSTM 结构详解
