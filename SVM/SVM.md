# SVM


![7e49cf765eb3680c85185bc30a9db196_r.jpg](https://pic3.zhimg.com/7e49cf765eb3680c85185bc30a9db196_r.jpg)
left- svm basic problem  
right- extension



## The optimation problem

$\max \frac{1}{\|w\|}\quad s.t., y_i(w^Tx_i+b)\geq 1, i=1,\ldots,n$

Notice: 由于这些 supporting vector 刚好在边界上，所以它们是满足 y(wTx+b)=1 （还记得我们把 functional margin 定为 1 了吗？），而对于所有不是支持向量的点，也就是在“阵地后方”的点，则显然有 __y(wTx+b)>1__  

事实上，当最优的超平面确定下来之后，这些后方的点就完全成了路人甲了，它们可以在自己的边界后方随便飘来飘去都不会对超平面产生任何影响。这样的特性在实际中有一个最直接的好处就在于存储和计算上的优越性, 例如, 如果使用100万个点求出一个最优的超平面，其中是supporting vector的有100个，那么我只需要记住这100个点的信息即可，对于后续分类也只需要利用这100个点而不是全部100万个点来做计算。（当然，通常除了K-Nearest Neighbor 之类的 Memory-based Learning算法，通常算法也都不会直接把所有的点记忆下来，并全部用来做后续 inference 中的计算。
不过，如果算法使用了Kernel方法进行非线性化推广的话，就会遇到这个问题了。


$y(w^Tx+b)=1$



So the problem is equal to:

$min \frac{2}{∥w∥}\quad s.t., yi(w^Tx_i+b)≥1,i=1,…,n$



Lagrange：
$L(w,b,α) = \frac{1}{2}w^tw\ −∑i=1 n αiyi(wTxi+b)−1$

$L(w,b,α) = \frac{1}{2}\left \| w \right \|^{2} – \sum_{n=1}^{n}α_{n}*\left \{ y_{n}\left ( w^{T}x^{n} +b \right ) – 1 \right \}$



首先要让L关于 w 和 b 最小化，我们分别令 ∂L/∂w 和 ∂L/∂b 等于零：
$θ(w)=maxαi≥0 \quad L(w,b,α)$

$∂L∂w=0\quad ⇒w=∑i=1nαiyixi \quad ∂L∂b=0 \quad ⇒∑i=1nαiyi=0$

$0=\sum_{n=1}^{N}a_{n}y_{n}$

$w=\sum_{n=1}^{N}a_{n}y_{n}x_{n}$



## Duality problem

$\underset{all \ a_{n}}{\max}L(a) = \sum_{n=1}^{N}a_{n} – \frac{1}{2}\sum_{n=1}^{N}\sum_{M=1}^{N}a_{n}a_{m}y_{n}y_{m}x_{n}^{T}x_{m}\ \\ s.t. \\ a_{n}\geq 0,\forall n,\sum_{n=1}^{N}a_{n}y_{n}=0$



