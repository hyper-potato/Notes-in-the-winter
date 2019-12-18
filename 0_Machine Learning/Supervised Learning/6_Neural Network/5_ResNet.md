- learning residuals like gradient boosting

- Shortcut: make sure infomation from point A from point B is consistent
  - identity block
  - conv block

but why learning residual is earier than direct mapping with shortcut? 

关于探究ResNet的由来，作者是基于先前的什么知识才提出残差网络的，咋一看感觉残差网络提出的很精巧，其实就是很精巧，但是现在感觉非要从残差的角度进行解读感觉不太好理解，**真正起作用的应该就是shortcut连接了，这才是网络的关键之处。**



### 对ResNet的解读

基本的残差网络其实可以从另一个角度来理解，这是从另一篇论文里看到的，如下图所示：

![](https://img-blog.csdn.net/20161125155120022)



残差网络单元其中可以分解成右图的形式，从图中可以看出，残差网络其实是由多种路径组合的一个网络，直白了说，残差网络其实是很多并行子网络的组合，整个残差网络其实相当于一个多人投票系统（Ensembling）。下面来说明为什么可以这样理解

### 删除网络的一部分
如果把残差网络理解成一个Ensambling系统，那么网络的一部分就相当于少一些投票的人，如果只是删除一个基本的残差单元，对最后的分类结果应该影响很小；而最后的分类错误率应该适合删除的残差单元的个数成正比的，论文里的结论也印证了这个猜测。
下图是比较VGG和ResNet分别删除一层网络的分类错误率变化

![](https://img-blog.csdn.net/20161125155957236)



ResNet分类错误率和删除的基本残差网络单元个数的关系：

![](https://img-blog.csdn.net/20161125160101250)

### ResNet的真面目
ResNet的确可以做到很深，但是从上面的介绍可以看出，网络很深的路径其实很少，大部分的网络路径其实都集中在中间的路径长度上，如下图所示：

![](https://img-blog.csdn.net/20161125160447695)





从这可以看出其实ResNet是由大多数中度网络和一小部分浅度网络和深度网络组成的，说明虽然表面上ResNet网络很深，但是其实起实际作用的网络层数并没有很深，我们能来进一步阐述这个问题，我们知道网络越深，梯度就越小，如下图所示

![](https://img-blog.csdn.net/20161125160830856)



而通过各个路径长度上包含的网络数乘以每个路径的梯度值，我们可以得到ResNet真正起作用的网络是什么样的，如下图所示

![](https://img-blog.csdn.net/20161125161016577)





我们可以看出大多数的梯度其实都集中在中间的路径上，论文里称为effective path。
从这可以看出其实ResNet只是表面上看起来很深，事实上网络却很浅。
所示ResNet真的解决了深度网络的梯度消失的问题了吗？似乎没有，ResNet其实就是一个多人投票系统。