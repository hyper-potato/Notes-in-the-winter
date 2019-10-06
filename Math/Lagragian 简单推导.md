# Lagrangian duality 简单推导

抛砖引玉, 说一下(Lagrangian) duality是怎么来的。先考虑下面的nonlinear programming:

![[公式]](https://www.zhihu.com/equation?tex=%5Cmin+%5C%7Bf%28%5Cmathbf%7Bx%7D%29%3A+g_i%28%5Cmathbf%7Bx%7D%29%5Cleq+0%2C%5C%3B+i%3D1%2C2%2C...%2Cm%5C%7D)			(1)



现在的问题是如何找到问题(1) 的最优值的一个最好的下界? 首先我们知道若方程组

![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Balign%7D%26f%28%5Cmathbf%7Bx%7D%29%3Cv%5C%5C%26g_i%28%5Cmathbf%7Bx%7D%29%5Cleq+0%2C+i%3D1%2C2%2C...%2Cm%5Cend%7Balign%7D)         									(2)

无解，则![[公式]](https://www.zhihu.com/equation?tex=v)是问题(1)的一个下界。

注意到方程组(2)有解可以推出对于任意的![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%5Clambda%7D%5Cgeq+%5Cmathbf%7B0%7D), 以下方程

![[公式]](https://www.zhihu.com/equation?tex=f%28%5Cmathbf%7Bx%7D%29%2B%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%5Clambda_ig_i%28%5Cmathbf%7Bx%7D%29%3Cv)														(3)

有解。

因此根据逆否命题，方程组(2)无解的充分条件是存在![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%5Clambda%7D%5Cgeq+%5Cmathbf%7B0%7D)，让方程(3)无解。方程(3)无解的充要条件是

![[公式]](https://www.zhihu.com/equation?tex=%5Cmin_%7B%5Cmathbf%7Bx%7D%7D+f%28%5Cmathbf%7Bx%7D%29%2B%5Csum_%7Bi%3D1%7D%5Em+%5Clambda_ig_i%28%5Cmathbf%7Bx%7D%29%5Cgeq+v)											(4)



因为我们要找最好的下界，所以这个时候的![[公式]](https://www.zhihu.com/equation?tex=v)和![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%5Clambda%7D)应该取

![[公式]](https://www.zhihu.com/equation?tex=v%3D%5Cmax_%7B%5Cbm%7B%5Clambda%7D%5Cgeq+%5Cmathbf%7B0%7D%7D%5Cmin_%7B%5Cmathbf%7Bx%7D%7D+f%28%5Cmathbf%7Bx%7D%29%2B%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%5Clambda_ig_i%28%5Cmathbf%7Bx%7D%29)								(5)

由此引入了dual problem. 

证明逻辑是根据式(5)取![[公式]](https://www.zhihu.com/equation?tex=v)和![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%5Clambda%7D), 则(4)成立，从而导出(3)无解，然后可以知道(2)无解，因此![[公式]](https://www.zhihu.com/equation?tex=v)是问题(1)的下界。