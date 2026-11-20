
![](img/f566c91d4b5d7749a68a08dd88a870c8.jpg)

序列 `V` 是上面的散点图中的黄色部分。`Beta` 是取值为 0 到 1 的另一个超参数。在上述例子中，取 `Beta = 0.9`。`0.9` 是一个很好的值，经常用于具有动量的 SGD 方法。我们可以这样对 `Beta` 进行直观理解：我们对序列后面的 `1 /（1- beta）` 的点进行近似平均。让我们看看 `beta` 的选择会对新序列 `V` 产生怎样的影响。

![](img/22006733594e989a897659033ed85757.jpg)

*`Beta` 取值不同时的指数加权平均结果。*

如我们所见，`Beta` 取值越小，序列 `V` 波动越大。因为我们平均的例子更少，因此结果与噪声数据更「接近」。随着 `Beta` 值越大，比如当 `Beta = 0.98` 时，我们得到的曲线会更加圆滑，但是该曲线有点向右偏移，因为我们取平均值的范围变得更大（`beta = 0.98` 时取值约为 50）。`Beta = 0.9` 时，在这两个极端间取得了很好的平衡。

## 数学部分

这个部分对你在项目中使用动量而言不是必要的，所以可以跳过。但这部分更直观地解释了动量是如何工作的。

让我们对指数加权平均新序列 `V` 的三个连续元素的定义进行扩展。

![](img/79dfbf68a8ca999a9c0022565cd9849d.jpg)

*`V`——新序列。`S`——原始序列。*

将其进行合并，我们可以得到：

![](img/93b3e5b4b5555fec0de6830bd64e14b6.jpg)

再对其进行简化，可得：

![](img/2eea51866f9d07792520d171d9a29647.jpg)

从这个等式中可以看出，新序列的第 `T` 个值取决于原始序列 `S` 的所有先前的数值 `1…t`。来自 `S` 的所有数值被赋了一定的权重。这个权重是序列 `S` 的第（`t-i`）个值乘以（`1- beta`）得到的权重。因为 `Beta` 小于 1，所以当我们对某个正数的幂取 `beta` 时，值会变得更小。所以序列 `S` 的原始值的权重会小得多，也因此序列 `S` 对序列 `V` 产生的点积影响较小。从某些角度来说，该权重小到我们几乎可以说我们「忘记」了这个值，因为其影响小到几乎无法注意到。使用这个近似值的好处在于当权重小于 `1 / e` 时，更大的 `beta` 值会要求更多小于 `1 / e` 的权值。这就是为什么 `beta` 值越大，我们就要对更多的点积进行平均。下面的图表显示的是与 `threshold = 1 / e` 相比，随着序列 `S` 初始值变化，权重变小的速度，在此我们「忘记」了初始值。

![](img/22073fc4e544c1824775d9173dab45ee.jpg)

最后要注意的是，第一次迭代得到的平均值会很差，因为我们没有足够的值进行平均。我们可以通过使用序列 `V` 的偏差修正版而不是直接使用序列 `V` 来解决这一问题。

![](img/169791438a30c39af820e08fc30dafff.jpg)

式中 `b = beta`。当 `t` 值变大时，`b` 的 `t` 次幂与零无法进行区分，因此不会改变 `V` 值。但是当 `t` 取值较小时，这个方程会产生较好的结果。但是因为动量的存在使得机器学习过程稳定得很快，因此人们通常会懒得应用这一部分。

## 动量 SGD 法

我们已经定义了一种方法来获得一些序列的「移动」平均值，该值会与数据一起变化。我们该如何将其应用于神经网络的训练中呢？它可以平均我们的梯度。我将在下文中解释它是如何在动量中完成的这一工作，并将继续解释为什么它可能会得到更好的效果。

我将提供两个定义来定义具有动量的 SGD 方法，这几乎就是用两种不同的方式表达同一个方程。首先，是吴恩达在 Coursera 深度学习专业化（`https://www.deeplearning.ai/`）的课程中提出的定义。他解释的方式是，我们定义一个动量，这是我们梯度的移动平均值。然后我们用它来更新网络的权重。如下所示：

![](img/1d07dea98933418d43e81af59f1a1411.jpg)

式中 `L` 是损失函数，三角形符号是梯度 w.r.t 权重,`α` 是学习率。另一种最流行的表达动量更新规则的方式不那么直观，只是省略了（`1 - beta`）项。

![](img/38c920fe98e8e9783045cbcfb87d107a.jpg)

这与第一组方程式非常相似，唯一的区别是需要通过（`1 - β`）项来调整学习率。

## Nesterov 加速渐变

Nesterov 动量是一个版本略有不同的动量更新，最近越来越受欢迎。在这个版本中，首先会得到一个当前动量指向的点，然后从这个点计算梯度。如下图所示：

![](img/259937ad91c0d5bf8430df3f541cc313.jpg)

Nesterov 动量可用下式定义：

![](img/87d97adac541cc45b595b55b9c44143a.jpg)

## 动量工作原理

在这里我会解释为什么在绝大多数情况下动量法会比经典 SGD 法更好用。

使用随机梯度下降的方法，我们不会计算损失函数的确切导数。相反，我们是对一小批数据进行估算的。这意味着我们并不总是朝着最佳的方向前进，因为我们得到的结果是「嘈杂的」。正如我在上文中列出的图表。所以，指数的加权平均可以提供一个更好的估计值，该估计值比通过嘈杂计算得到的结果更接近实际值的导数。这就是动量法可能比传统 SGD 更好的原因之一。

另一个原因在于沟谷（ravine）。沟谷是一个区域，在其中，曲线在一个维度比另一个维度陡得多。在深度学习中，沟谷区可近似视为局部最低点，而这一特性无法用 SGD 方法得到。SGD 倾向于在狭窄的沟谷上摆动，因为负梯度将沿着陡峭的一侧下降，而非沿着沟谷向最优点前进。动量有助于加速梯度向正确的方向前进。如下图所示：

![](img/73e35b79fa7a28bafe5fb0c79c9381de.jpg)

*左图——没有动量的 SGD，右图——有动量的 SGD（来源：`https://www.willamette.edu/~gorr/classes/cs449/momrate.html`）*

## 结论

希望本节会提供一些关于具有动量的 SGD 方法是如何起作用以及为什么会有用的想法。实际上它是深度学习中最流行的优化算法之一，与更高级的算法相比，这种方法通常被人们更频繁地使用。

## 参考资源

* `neuralnetworksanddeeplearning.com`（`http://neuralnetworksanddeeplearning.com/chap1.html`）：一本关于基本知识的很好的在线书籍。关于神经网络背后的理论。作者以一种很好的方式解释了你需要知道的数学知识。它也提供并解释了一些不使用任何深度学习框架从零开始编写神经网络架构的代码。

* Andrew Ng 的深度学习课程（`https://www.coursera.org/specializations/deep-learning`）：coursera 上的课程，也是有关学习神经网络的。以非常简单的神经网络例子开始，逐步到卷积神经网络以及更多。

* 3Blue1Brown（`https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw`）：YouTube 上也有一些能够帮助你理解神经网络和线性代数的很好的视频。它们展示了很棒的可视化形式，以及以非常直觉的方式去理解数学和神经网络。

* Stanford CS231 课程（`http://cs231n.stanford.edu/`）：这是关于用于视觉识别的卷积神经网络的课堂，可以学到很多关于深度学习和卷积神经网络的具体内容。

## 参考文献

[1] Ning Qian. On the momentum term in gradient descent learning algorithms . Neural networks : the official journal of the International Neural Network Society, 12(1):145–151, 1999

[2] Distill, Why Momentum really works（`https://distill.pub/2017/momentum/`）

[3] deeplearning.ai

[4] Ruder(2016). An overview of gradient descent optimization algorithms. arXiv preprint arXiv:1609.04747

[5] Ruder (2017) Optimization for Deep Learning Highlights in 2017.（`http://ruder.io/deep-learning-optimization-2017/index.html`）

[6] Stanford CS231n lecture notes.（`http://cs231n.github.io/neural-networks-3/`）

[7] fast.ai（`http://www.fast.ai/`）

原文链接：

**第一部分**：`https://towardsdatascience.com/how-do-we-train-neural-networks-edd985562b73`

**第二部分**：Stochastic Gradient Descent with momentum (`https://quip.com/tJ7sAxbyrwZg`)

`https://towardsdatascience.com/stochastic-gradient-descent-with-momentum-a84097641a5d`

**本文为机器之心编译，转载请联系本👸号获得授权。**

---

**加入机器之心（全职记者/实习生）：`hr@jiqizhixin.com`**

**投稿或寻求报道：`content@jiqizhixin.com`**

**广告&商务合作：`bd@jiqizhixin.com`**
