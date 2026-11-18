
# 自编码器原理与 TensorFlow 实现

## 自编码器的基本概念

所以为什么自编码器是一系列方法呢？因为我们仅有的约束条件是其输入层和输出层具有同样的维度，在两者之间，我们可以创建任何我们想要的可以最好地编码我们的高维数据的结构。

自编码器始于一些随机的低维表征（`z`）并会通过梯度下降改变其输入层和隐藏层以及隐藏层和输出层之间连接的权重，从而找到它们的解。

现在，我们对自编码器已经有了一定了解，因为我们可以控制该网络的内部，我们可以让编码器能挑选出特征之间的非常复杂的关系。

自编码器还有一个加分项。因为在训练结束时，我们有与隐藏层的连接权重，所以我们可以在特定的输入上训练，如果后面我们遇到了另一个数据点，那么我们无需重新训练就可以使用这些权重进行降维——但这种操作要小心，只有当新数据点与我们训练所用的数据点类似时这才有效。

## 成本函数与模型灵活性

在这种情况下，探索自编码器的数学可能很简单，但却没什么用处，因为对于每一种架构和我们选择的成本函数，其数学形式可能是不同的。

但如果我们想一想自编码器权重的优化方式，我们会理解我们定义的成本函数具有非常重要的作用。

因为自编码器会使用成本函数来确定其预测结果的质量，那么我们就可以使用这个功能来强化我们希望实现的东西。

不管我们想要的是欧几里得距离还是其它测量，我们都可以通过成本函数、使用不同的距离方法、使用不对称函数和其它方法而将其反映到编码的数据上。

而且因为自编码器本质上是神经网络，所以它还具有更大的力量，我们甚至可以在训练时给类和样本加权，从而为数据中的特定现象提供更大的重要性。

这能给我们压缩数据的方式提供很大的灵活性。

自编码器非常强大，而且在一些案例中能实现比其它方法更好的结果（谷歌搜一下「PCA vs Auto Encoders」，你就知道），所以它们肯定是一种有效的方法。

## 基于 TensorFlow 的鸢尾花数据集实现

让我们用 TensorFlow 实现一个基于鸢尾花数据集的基本自编码器，并且绘图。

### 代码实现

同样，我们将其分成了 `fit()` 和 `reduce()`。

```py
def fit(self, n_dimensions):
    graph = tf.Graph()
    with graph.as_default():

        # Input variable
        X = tf.placeholder(self.dtype, shape=(None, self.features.shape[1]))

        # Network variables
        encoder_weights = tf.Variable(tf.random_normal(shape=(self.features.shape[1], n_dimensions)))
        encoder_bias = tf.Variable(tf.zeros(shape=[n_dimensions]))

        decoder_weights = tf.Variable(tf.random_normal(shape=(n_dimensions, self.features.shape[1])))
        decoder_bias = tf.Variable(tf.zeros(shape=[self.features.shape[1]]))

        # Encoder part
        encoding = tf.nn.sigmoid(tf.add(tf.matmul(X, encoder_weights), encoder_bias))

        # Decoder part
        predicted_x = tf.nn.sigmoid(tf.add(tf.matmul(encoding, decoder_weights), decoder_bias))

        # Define the cost function and optimizer to minimize squared error
        cost = tf.reduce_mean(tf.pow(tf.subtract(predicted_x, X), 2))
        optimizer = tf.train.AdamOptimizer().minimize(cost)

    with tf.Session(graph=graph) as session:
        # Initialize global variables
        session.run(tf.global_variables_initializer())

        for batch_x in batch_generator(self.features):
            self.encoder['weights'], self.encoder['bias'], _ = session.run([encoder_weights, encoder_bias, optimizer],
                feed_dict={X: batch_x})
```

这里没什么特别要说的，代码本身就已经给出了解释。我们可以看到在偏置中的编码器权重，然后我们可以在下面的 `reduce()` 方法中给该数据降维。

```py
def reduce(self):
    return np.add(np.matmul(self.features, self.encoder['weights']), self.encoder['bias'])
```

没错，就是这么简单 :)

### 可视化与调优

让我们看看它的效果（批大小：50，1000 epoch）。

![](img/603ebe18f97f64b8c96af23d4a451623.jpg)

*图 7：这个简单自编码器在鸢尾花数据集上的输出*

我们可以继续调整批大小、epoch 数和不同的优化器，甚至无需改变架构我们就能得到不同的结果。

注意这里我给超参数随便选择了一些值，在真实场景中，我们需要通过交叉验证或测试数据来检验我们的结果，并借此找到最优的设置。

## 结语

这样的文章往往会通过某种形式的对比表格收尾，看看它们长处短处之类的。但那正好不是我想做的事情。

我的目标是深入到这些方法内部，让读者可以自己明白它们各自的优势和劣势。

我希望你喜欢这篇文章，并且能从中获益。现在回到文章开始处的问题，你知道它们的答案了吗？

![](img/772b30b2a3f8d55dd21a3f1d22cb6891.jpg)

`https://reinforce.io/blog/introduction-to-tensorforce/`

本文为机器之心编译，转载请联系本👸号获得授权。

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@jiqizhixin.com**

**投稿或寻求报道：editor@jiqizhixin.com**

**广告&商务合作：bd@jiqizhixin.com**

**点击阅读原文，查看机器之心官网↓↓↓**
