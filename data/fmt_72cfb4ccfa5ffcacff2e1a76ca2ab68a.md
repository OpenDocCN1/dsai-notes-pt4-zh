
在这里我们聊一聊怎么初始化参数吧。你需要用 `tf.contrib.layers.xavier_initializer(seed=0)` 来初始化权重/滤波器 `W1` 和 `W2`。你不需要担心偏重值因为很快你就能发现 `TensorFlow` 函数已经解决了这一点。注意，你只需初始 `conv2d` 函数的权重/滤波器，`TensorFlow` 会自动初始全连接部分的层。我们会在之后的作业里看到更多。

练习：实现 `initialize_parameters()`。每一组滤波器/权重的维度已经提供给大家。记住，在 `TensorFlow` 里初始化一个形状为 `[1,2,3,4]` 的参数 `W` 用：

```
W = tf.get_variable("W", [1,2,3,4], initializer = ...)
```

More Info. 更多的信息：

```python
# GRADED FUNCTION: initialize_parameters

def initialize_parameters():
    """
    Initializes weight parameters to build a neural network with tensorflow. The shapes are:
    W1 : [4, 4, 3, 8]
    W2 : [2, 2, 8, 16]
    Returns:
    parameters -- a dictionary of tensors containing W1, W2
    """
    tf.set_random_seed(1) # so that your "random" numbers match ours

    ### START CODE HERE ### (approx. 2 lines of code)
    W1 = tf.get_variable("W1", [4, 4, 3, 8], initializer = tf.contrib.layers.xavier_initializer(seed=0))
    W2 = tf.get_variable("W2", [2, 2, 8, 16], initializer = tf.contrib.layers.xavier_initializer(seed=0))
    ### END CODE HERE ###

    parameters = {"W1": W1,
                  "W2": W2}
    return parameters
```

### 3.3 Keras 教程：幸福家（Happy House）

这一部分讲了一个作业（幸福家），怎样用 `Keras` 建模型、总结、用自己的图片去测试其他非常有用的 `Keras` 函数。在这里我们着重讲一下幸福家是什么：

#### 3.3.1 幸福家

下一次放假旅行，你决定和你的 5 个在学校认识的朋友一起度过一周。这附近有一个非常方便的房子可以做很多事情。但最重要的福利是在房子里的时候每个人都必须要快乐。所以每个想进房间的人都要提供他们现在的幸福情形。

![](img/d92fb16f7d690ab58fbb5a55b863f09f-fs8.png)

作为一个深度学习专家，要确保「开心」这一规则被坚决的执行了，你要去建一个算法来通过前门的摄像头得到的照片来检查这个人是不是开心。

你已经收集到了你和朋友在前门照的照片，数据库已经被标注了。0 代表不开心，1 代表开心。

![](img/27c64b017dd510444af554dd7aa3fd73-fs8.png)

跑一下下面的程序让数据库更加标准化，然后学习一下它的形态。

```python
X_train_orig, Y_train_orig, X_test_orig, Y_test_orig, classes = load_dataset()

# Normalize image vectors
X_train = X_train_orig / 255.
X_test = X_test_orig / 255.

# Reshape
Y_train = Y_train_orig.T
Y_test = Y_test_orig.T

print("number of training examples = " + str(X_train.shape[0]))
print("number of test examples = " + str(X_test.shape[0]))
print("X_train shape: " + str(X_train.shape))

print("Y_train shape: " + str(Y_train.shape))

print("X_test shape: " + str(X_test.shape))
print("Y_test shape: " + str(Y_test.shape))

#output
number of training examples = 600
number of test examples = 150
X_train shape: (600, 64, 64, 3)
Y_train shape: (600, 1)
X_test shape: (150, 64, 64, 3)
Y_test shape: (150, 1)
```

幸福家数据库的细节：

*   图片数据大小为 (64, 64, 3)  
*   训练数据：600
*   测试数据：150

现在去解决「快乐」挑战吧！

### 3.4 残差网络

这一部分介绍了非常深的深度神经网络的问题，如何构建残差模块、残差连接、卷积块等，并组合它们而建造第一个残差网络模型以及用自己的图片去实验。

残差网络可以解决一些非常深的深度神经网络所具有的问题，我们在这里着重谈一下非常深的深度网络的问题。

近几年，神经网络变得越来越深了，最前沿的网络有几层的，也有超过一百层的。

深度神经网络最主要的优势是它可以表示非常复杂的函数。它也可以从不同的抽象等级去学习从边缘特征（浅层）到复杂的特征（深层）。但是，用一个更深的神经网络不总是有效。一个大缺点是训练模型的时候梯度会消失：非常深的网络经常会非常快地梯度下降到 0。这样会让梯度下降变得不可忍受的慢，因为每一次只会更新一点点。更具体地，在梯度下降的时候，当你反向传播从最后一层一直到第一层的时候，每一步都在乘以权重矩阵，所以梯度会以指数函数的速率下降到 0（或者在有些极少的情况里，梯度以指数函数的速率产生爆炸）。

在训练的时候，你也能看到前面层的梯度数值非常快地下降到 0。

![](img/fd29918565e1e306c410105caa35e8ba-fs8.png)

你现在可以用残差网络解决这个问题。

### 3.5 用 `YOLOv2` 检测车辆

这一部分讲了：问题叙述、`YOLO`、模型细节、用一个门槛来过滤一个班的分数、非最大抑制、包装过滤器、在图片上测试 `YOLO` 模型、定义类、转折点和图片大小、加载一个训练好的模型、转化输出模型到一个可用的边界盒张量、过滤盒及对图片跑计算图。

在这里我们讲一下问题是什么：

你现在在做一个自动驾驶车，作为一个重要的部分，你想先建一个车辆检测系统，当你开车时候这个系统会每隔几秒照一下前面的路。

![](img/42efbedca8eb42048e209aecf4a9198f-fs8.png)

你现在已经收集了所有的这些图片到一个文件夹里然后也已经用框标注出了每一个你可以找到的车。这里是例子：

![](img/32e49c77f23684ce19d32f90a8bd859e-fs8.png)

`p` 代表了你有多自信圈出来的是什么，`c` 代表了你认为圈出来的是什么。

如果你想让 `YOLO` 认出 80 种类别, 你可以让 `c` 表示 1 到 80 中的一个数，或者 `c` 是一个 80 长度的矢量。视频课里已经用了后一种表示。在这个笔记里，我们两种都用了，这取决于哪种更好用。在这个练习里，你将要学习 `YOLO` 怎样工作，怎样运用这个区检测车。因为 `YOLO` 训练的时候非常耗费计算量，我们将会加载训练好的权重来用。

### 3.6 快乐家中的脸部识别

在这一张里我们可以看到：简略脸部识别，脸部图片编成一个人 128 维度的矢量，使用 `ConvNet` 来计算编码，三联损失，加载训练好的模型和运用训练好的模型。

我们着重介绍一下简略脸部识别：

在脸部识别里，给你两张图片然后你必须告诉我两个人是否是同一个。最简单的做法是比较两张图片中的每一个像素，如果两张原始图片的比较结果小于一个门槛值则可以说两张图片是一个人。

![](img/ee39dc81b493656bc0119ff1b50de21c-fs8.png)

当然，这个算法的性能非常差，因为像素的值会随着光的改变，方向的改变，甚至镜像改变头的位置而剧烈的改变。你将看到除了用原始图片你更愿意编码一个 `f(image)` 来比较每一个像素这样会给你一个关于两张照片是不是一个人更准确的答案。

### 3.7 通过神经风格迁移生成艺术

在这一章节中，我们将尝试实现神经风格迁移，并使用算法生成新颖的艺术风格图像。在神经风格迁移中，重点是我们需要优化成本函数获得像素的值。如下所示，我们使用某张图像的风格，并迁移到需要这种风格的图像中：

![](img/336e54b7ad6bd053bdf95a9b257900da-fs8.png)

神经风格迁移主要使用预训练的卷积神经网络，并在它的顶部构建新的层级。这种使用预训练模型，并将其应用到新任务的方法可以称为迁移学习。卷积网络的迁移学习非常简单，一般来说，我们可以将最后几个分类层的权重随机初始化，再在新的数据集上训练而快速获得优秀的性能。

在 `NST` 原论文中，我们会使用 `VGG-19` 网络，它会预先在 `ImageNet` 上实现训练。因此在 Wan Zhen 的笔记中，我们可以运行以下命令下载模型及参数：

```python
model = load_vgg_model("pretrained-model/imagenet-vgg-verydeep-19.mat")
print(model)
```

然后使用 `assign` 方法将图像作为模型的输入：

```python
model["input"].assign(image)
```

随后，我们就能使用以下代码获取特定层级的激活值：

```python
sess.run(model["conv4_2"])
```

在神经风格迁移中，重点是以下三个步骤：

*   构建需要迁移风格的图片损失函数 `J_content(C, G)`
*   构建风格损失函数 `J_style(S, G)`
*   将它们组合为最终的损失函数 `J(G) = α J_content(C, G) + β J_style(S, G)`

使用这样的损失函数，最终我们能以迁移学习的方法实现神经风格迁移。![](img/34deb7ee40ccfed1601e199447af9375-fs8.png)

本文为机器之心整理，转载请联系原作者获得授权。

**加入机器之心（全职记者/实习生）：hr@jiqizhixin.com**

**投稿或寻求报道：editor@jiqizhixin.com**

**广告&商务合作：bd@jiqizhixin.com**
