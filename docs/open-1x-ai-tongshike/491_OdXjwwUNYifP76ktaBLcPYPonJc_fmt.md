
# Task1：赛事初探——降水预测模型搭建过程概述

> 来源：[https://datawhaler.feishu.cn/docx/Op4fdeK0NonIFmxzSdPcU8RCnsd](https://datawhaler.feishu.cn/docx/Op4fdeK0NonIFmxzSdPcU8RCnsd)

> 教程贡献者说：

> 天气预报、降水预测...... 这些高大上的场景, 你是否曾想过可以通过数据分析和深度学习的手段来实现并能取得十分精准的预测效果？AI时代，只需要数据与模型, 你也可以从小白摇身一变成为气象预测专家! 想知道具体应该怎么通过数据和代码实现精准天气预报吗, 别急, 本次课程将逐步为你揭晓~

> 在本次课程中, 你将收获对机器学习、深度学习的初步认识，对常用深度学习框架`pytorch`的认识, 对极端降水预测赛题的初步了解。

> 那么，让我们马上开始，开启今天学习之旅~

## Part1 什么是机器学习, 什么是深度学习？

机器学习是人工智能的一个分支，它使计算机系统利用数据来不断改进性能，而无需进行明确的编程。

通俗来说就是, 机器学习像一个学生, 通过给定的教材, 不断地学习教材内容，最终可以在期末考试或者综合实践中取得优异的成绩。

核心思想：通过算法和统计模型，机器学习允许计算机从经验中学习，识别模式，并做出预测或决策。

+ 监督学习：算法从标记的训练数据中学习，这些数据包含了输入和期望的输出。

+ 无监督学习：算法处理未标记的数据，试图找出数据中的结构和模式。

同时，通过任务的输出是否离散, 机器学习的任务又可以划分为分类与回归

![](img/f1858b3de5e9ebd74b0b1043747f1cf4.png)

深度学习则是机器学习中神经网络算法的进阶版。神经网络算法通过模拟人脑神经元的信息传递，逐层递进, 从而形成一个机器学习模型，而深度学习有着更深的层数, 有对大规模数据(例如上百万级)更好的学习效果，这个特点使得深度学习在数据量暴增的时代脱颖而出。

神经网络示意图如下:

![](img/6b46ff2d47df760ebb3315864856cf01.png)

## Part2 什么是`pytorch`？

`PyTorch`是由Meta AI(Facebook)人工智能研究小组开发的一种基于Lua编写的Torch库的Python实现的深度学习库，目前被广泛应用于学术界和工业界。通过`pytroch`, 我们可以自由的搭建神经网络模型, 使之适配我们所需要的任务。

在我们的`baseline`中，有几个工具类十分重要, 构建他们的过程, 都少不了`pytorch`的帮助

1. 构建数据集的类`Dataset`

2. 数据加载类的`Dataloader`

3. 定义模型的类`Model`

`pytorch`是一个非常好用的工具, 以便我们自由的构建我们的模型、数据集，这让我们可以更方便的尝试各种网络模型或者优化方法, 同时又不需要大量的重复编程实现；另外, `pytorch`对GPU硬件有很好的支持, 通过GPU对我们的模型训练进行加速, 可以节约大量的时间开销。可以说, 作为一款深度学习框架, `pytorch`目前在学术界和工业界都有着极高的地位.

![](img/d1d567aaaceeeca50728e1acbbe3592a.png)

## Part3 赛题初探, 如何使用深度学习实现搭建降水预测模型？

降水预测模型是一类时间序列相关的问题， 是一个有标注的监督学习的回归任务, 是使用在时间序列上前一部分的数据来预测后一部分的数据。通俗来说, 时间序列预测模型目的在构建一个全时段可用的时间序列函数, 根据输入可以精准的得到目的的输出, 比如本次赛题中的降水预测模型.

那么我们如何去搭建一个预测模型呢?

1. 首先我们需要做数据处理, 如果打开下载后的数据文件我们可以看到, 里面有`feature`特征文件和`groundtruth`真值文件, 因此我们首先要建立对应`feature`和`groundtruth`数据之间的关系, 这可以从`baseline`里看到具体的构建方法.

2. 接着, 如果把降水预测模型看作一个函数, 我们需要尽可能找到一个最好拟合降水预测过程的函数. 这时, 对数据有着良好拟合效果的深度学习就派上了用场：通过`pytorch`, 我们可以搭建神经网络, 尽可能去好的在我们的数据上做拟合, 通过一系列尝试我们就得到了拟合效果最好的模型.

3. 最后, 我们需要在测试数据上验证模型的效果.

大致通过这三步, 一个简单的预测模型就可以被我们搭建起来了.

![](img/08b8aa6397cdf581cf74005c48a30497.png)

![](img/3372329ca17d4fcabafef3f0af697571.png)

> 温馨提示：`baseline`中的代码仅下载了 weather.round1.train.gt.2019-2021.zip 和 weather.round1.test.zip文件，后续上分可能需要使用更多特征和数据，可在`Notebook`中通过如下命令下载，如果需要在命令行运行，去掉每个命令最开始的 ! 即可。

> 另外还可以使用 `!axel -n 5 -o` 命令实现更快捷的下载

```
!wget -O weather.round1.train.gt.2019-2021.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.gt.2019-2021.zip?Expires=1721852797&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=%2B4JoTbsP%2FQuqmG%2FZbWWH2oYZPRM%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2021.2.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2021.2.zip?Expires=1721852870&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=nHf8TSnRfFtcMDcaIjLG6HPh%2FvI%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2020.2.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2020.2.zip?Expires=1721852901&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=Q3ox8S1BH2O1iGlvlS73fqqUyfk%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2021.1.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2021.1.zip?Expires=1721852947&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=qpA4yqb6X5oBxF6JLkJaXi2RKPE%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2020.1.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2020.1.zip?Expires=1721852982&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=QTXWzcOMtwMhVThzo9oeOf7GdQQ%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2019.1.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2019.1.zip?Expires=1721853153&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=4gthkBT9KHya%2FIFwjyFIx69DN7o%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.train.ft.2019.2.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.train.ft.2019.2.zip?Expires=1721853188&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=egty%2BptJf5Xc2cjIOc3dmInuauA%3D&amp;response-content-disposition=attachment%3B%20&#x27;
!wget -O weather.round1.test.zip &#x27;https://tianchi-race-prod-sh.oss-cn-shanghai.aliyuncs.com/file/race/documents/532234/bigFile/weather.round1.test.zip?Expires=1721853230&amp;OSSAccessKeyId=LTAI5t7fj2oKqzKgLGz6kGQc&amp;Signature=AGbaVGl%2BC7EdWBlKKnOkcJ5hJu0%3D&amp;response-content-disposition=attachment%3B%20&#x27;
```

## 上分思路

相信各位优秀的小伙伴们都已经完成了赛事速通，也获取到了第一个成绩, 恭喜各位~

那么接下来, 大家肯定对榜单上名列前茅的朋友们的方案十分感兴趣——他们是怎么做到的呢？

助教老师给大家一些小思路和方向, 大家可以先自由思考一下, 如何能获取到更优异的成绩呢.

1. 数据

2. 模型

3. 参数设置

4. 还有许多待揭晓的方法~

在后续课程中, 助教老师们将为大家揭示如何获取到更优异的成绩的秘诀，请各位期待！

欢迎将自己的实验添加到放在打卡链接中, 我们助教团会评审大家的作业, 如果有让我们眼前一亮的作品, 就有机会成为优秀学习者~

## 脑洞大开

恭喜大家顺利完成了本次的task，接下来有几个问题需要大家思考并给出你的答案~

1. 在`baseline`的基础上, 你会调整哪些部分, 以获取到更高的分数？

2. `baseline`中数据处理部分, 是否遇到了某些问题？你认为可以在哪里做一下提升和改进？

3. 模型训练部分，是否有哪些不合理或者需要提升的地方, 如果你来实现这一部分, 你会对哪里改进？

## 来个小总结

通过本次的学习, 大家已经对搭建一个降水预测模型有了初步的了解，对之前尝试的`baseline`也有了大致的认识, 明白了搭建一个降水预测模型的步骤是怎样的；同时, 也对一些基本的机器学习和深度学习知识, `pytorch`基础有了相关的了解，为我们之后的提升扫清了障碍。

那么接下来, 请再接再厉，向着更高的成绩冲冲冲~

## 继续迎接后续的挑战吧~

> 学有余力还可以往前学习哟~

<https://datawhaler.feishu.cn/sync/DScSdpeF4sY0cGbwBxRcETT7n8g>

> 本教程主要由 Chris 和 荞麦 写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> 注：本文采用署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
