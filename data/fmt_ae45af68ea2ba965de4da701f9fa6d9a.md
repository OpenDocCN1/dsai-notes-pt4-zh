
反向传播的实现是 OK 的！这里的差值是 3.0220555297630148e-09

## 结论

以下是一些关键点：

* 双边形式的数值梯度在逼近解析梯度时效果比单边形式的数值梯度更好。
* 由于梯度检验的运行很慢，因此：
  * 进行梯度检验时，只使用一个或少数样本；
  * 在确认反向传播的实现代码无误后，训练神经网络时记得取消梯度检验函数的调用。
* 如果使用了 drop-out 策略，（直接进行）梯度检验会失效。可以在进行梯度检验时，将 `keep-prob` 设置为 1，训练神经网络时，再进行修改。
* 通常采用 `e=10e-7` 作为检查解析梯度和数值梯度间差值的基准。如果差值小于 10e-7，则反向传播的实现代码没有问题。

幸运的是，在诸如 `TensorFlow`、`PyTorch` 等深度学习框架中，我们几乎不需要自己实现反向传播，因为这些框架已经帮我们计算好梯度了；但是，在成为一个深度学习工作者之前，动手实现这些算法是很好的练习，可以帮助我们理解其中的原理。![](img/2d1c94eb4a4ba15f356c96c72092e02b-fs8.png)

源代码地址：`https://github.com/ImadDabbura/blog-posts/blob/master/notebooks/Coding-Neural-Network-Gradient-Checking.ipynb`

原文链接：`https://imaddabbura.github.io/blog/machine%20learning/deep%20learning/2018/04/08/coding-neural-network-gradient-checking.html`

**本文为机器之心编译，转载请联系本👸号获得授权。**

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@jiqizhixin.com**

**投稿或寻求报道：content@jiqizhixin.com**

**广告&商务合作：bd@jiqizhixin.com**
