
![](img/ab3b011af3aba28a8a0d79d4291fb6b1.png)

### 2.3.3 跑几个 demo

**Ghost 的 case**

为了方便与前文进行对比，我选择 ghost 的数据集进行训练，下图展示了不同的 “训练 step“ x ”lora 权重” 的效果：

![](img/5d5d6b40d1930b8aa98481c7987594f9.png)

结论：

1. 随着训练的进行，lora 也能够逐渐认识 ghost，并且在训练过程中也能去除掉文字。
2. 权重适中即可，理论上训练完美的情况下权重==1 是合理的（因为在训练过程就是 1）；如果训练不充分（如第二行），可适当调大 lora 权重（但过大会让结果糊掉），反之如果过拟合，可适当调小 lora 权重。
3. 个人经验：如果要进行更多的控制操作（如 ControlNet，复杂的 prompt 等），建议把权重调小一些，以此来增大泛化性能
4. 像比于前文的 Dreambooth，lora 模型对于背景细节的控制能力较弱。好处是不容易对背景过拟合，坏处是可能需要对背景进行更多调教。
5. 尝试了不同的种子，生成图片的多样性较低（相对于 Dreambooth）

**星瞳的 case**

同样的方式，我们在星瞳的例子上进行尝试：

![](img/729b6aa6286ff976885a1e562135e16f.png)

在星瞳的例子中，生成图中有不少“类文字”的内容，这是因为训练样本中有很多图片中就包含文字，在 ghost 训练样本中文字被我手动抹去了。这也从另一方面说明了 lora 与 dreambooth 的一个不同点：

1. dreambooth 通过“唯一标识符”来学习样本，对细节把控更加精准。只有训练样本中出现频率高且变化不大的元素，才能被精准学习到（比如 CODM 的 ghost 和 logo、背景风格、星瞳的脸）
2. lora 对训练样本的学习更加泛化，训练样本中出现频率没那么高的内容也可以被学习到，但是对这部分信息学习的不准确，（比如星瞳身旁的文字，以及 ghost 身后的背景）

总结起来可以理解为：**dreambooth 学习的是，仔细对比不同样本中同时出现的特定元素；而 lora 学习的是，大概看一眼不同样本中同时出现的大概内容。**

**多 concept 融合的 badcase**

这里我同样尝试下 lora 的多 concept 是否表现更好，在数据目录下分别创建了 ghost 和星瞳两个 concept，并进行训练。但是在训练完成后，同样出现了 concept 融合的情况：

![](img/477407cd708fe0ed7d1042d877cda023.png)

关于 concept 融合的一些经验，目前网上的讲解不多，我看到比较好的是这篇：Lora 人物训练（多 concept）导论 ，他利用多 concept 来对同一个角色进行换装，concept 之间的差异较小。

【补充说明】：上边这篇资料中提到，秋叶的脚本无法调整`keep_tokens`，但实际上是可以的。在秋叶脚本的`train_util.py`中，`add_dataset_arguments`这个函数里可以看到`keep_tokens`参数的定义。当然我们也就可以在`launch`的命令中加入这个参数。

总的来说，concept 融合是比较难以避免的问题，因为从原理上看，模型就是把多个 concept 的数据放在一起直接进行训练的，模型在学习过程中难免会学混。（没有像 CLIP 模型那样采用了对比学习的方法，个人推测，如果采用对比学习的方法，再对错误配对的样本进行一定的惩罚，也许可以解决这个问题）

另外，如果对每个 concept 单独训练一个 lora 模型，然后在推理时一起应用，也会出现比较严重的融合现象。因为实际应用时，时先将所有 lora 模型加和到大模型上，在进行生成的。下面是同时使用 2 个 lora 生成的 badcase：

![](img/d78bbb99f2bdabd9fb24bd2d21a2dd6a.png)

对此，我总结了一些个人的经验：

1. concepts 之间如果是同类主体（比如两个都是人脸），融合风险较大；反之如果是不同主体（一个人脸，一个是装备），融合风险较低
2. 训练样本，要尽可能保证非目标主体内容尽可能多样化（比如目标主体是人脸，就尽可能有多样的背景、动作等）
3. 如果确实有比较强的多主体出图的需求，建议尝试 textual-inversion 的训练方法，这样不会把多个主体的梯度同时更新到共有的模型上。

所以目前的结论就是：如果要训练多个 concept，只有两种办法：

1. 准备足够精良的样本，同时包含两个 concept，并定义此为一个新的 concept（比如 C 站上 233 娘的那个 lora，应该就是把 22 娘和 33 娘两张脸当作一个共同的 concept 来学习）
2. 进行更加细致的样本、参数、训练方式（如 textual-inversion、对比学习等等）的调整和尝试。至少像我这样用默认参数和低质样本简单跑一下是不行的。

## 2.4 LoRA 的变体：LyCORIS

### 2.4.1 LyCORIS 是什么

[**Ly**](https://github.com/KohakuBlueleaf/LyCORIS)CORIS（Lora beYond Conventional methods， Other Rank adaptation Implementations for Stable diffusion） 是一些在 LoRA 的思想基础上，对具体网络结构的改进。

> A project for implementing different algorithm to do parameter-efficient finetuning on stable diffusion or more.

从前面代码中可以了解到，LoRA 实际上是对原始模型中每个层，都创建了 2 层线性全连接层，并在该层输出的时候加上去。那么如果我们创建的新结构不是 2 层线性全连接，而是其他什么结构，是否会有更好的效果呢？这就是 LyCORIS 项目研究的内容。

具体来说，截至目前（2023.04.04）LyCORIS 包含了 2 种方法（也就是[2 种不同的](https://github.com/KohakuBlueleaf/LyCORIS/blob/main/Algo.md)结构来替代 2 层全连接），分别为 LoCon 和 LoHa。这是官方的说明文档 ，下面我们分别简单介绍：

【补充说明】：与 lora 不同，lycoris 的脚本不放在`lora-scripts/sd-scripts/networks`下，而是在安装过程中直接集成在 python 环境中（具体在`lora-scripts/install.ps1`中的`pip install --upgrade lion-pytorch lycoris-lora`），因此其脚本保存在`lora-scripts/venv/Lib/site-packages/lycoris`下面。

**LoCon 介绍**

模型原理在官方的说明中已经介绍的很清楚了，其实就是用两个 Conv 操作，代替原来的两个矩阵相乘（`Wa`和`Wb`相乘）的操作：

![](img/26e16c4142bf29385410ad8e963ef47a.png)

下面我们浅浅的看一下关键代码，模型脚本位置：`lora-scripts/venv/Lib/site-packages/lycoris/locon.py`

本质上是把 LoRA 的 2 层全连接替换成了带卷积结构的网络，最终该层输出为原始模型输出+卷积结构输出*一些因子。

![](img/df47f0286ea582b33de87560842f810d.png)

**LoHa 介绍**

LoHa 方法，本质上是在 LoCon 的基础上，对`Wa`和`Wb`进行矩阵分解，其目的是为了提升结果矩阵`W`的秩的上限。这基于一种假设，即秩矩阵的秩越高，通常来讲其信息承载的能力越高。

![](img/d530a28b2c159af5985d292515cc987c.png)

下面我们浅浅的看一下关键代码，模型脚本位置：`lora-scripts/venv/Lib/site-packages/lycoris/loha.py`

本质上是依然是采用了 LoCon 的卷积操作，只不过其中参数不是直接初始化的大矩阵，而是通过初始化的小矩阵进行叉乘而得到的大矩阵。

![](img/e584103a1abee7202d6c86d603e1099c.png)

![](img/6ef1f486c646f2545277b4f029aae5db.png)

### 2.4.2 LyCORIS 实践

#### 2.4.2.1 使用方法

想要使用 lycoris 模型，需要安装 lycoris 插件：https://github.com/KohakuBlueleaf/a1111-sd-webui-locon

然后就可以将 lycoris 模型和 lora 模型一样，加入到 prompt 中来使用了

![](img/a563f828090b1cbcb4e1caf5004ba7a4.png)

而且也可以在“X/Y/Z plot”脚本中，对比不同模型的结果：

![](img/7640778ed3fc0a3179d8d8f3318f3021.png)

【补充说明】：目前的 lycoris 插件不支持 addtional network 插件，所以无法通过 addtional network 插件使其生效。（但是我亲自试用了下，locon 是可以生效的，但是 loha 是没生效）

#### 2.4.2.2 Demo 展示和分析

**LoCon - Ghost**

![](img/e2b83472f02a6c488ff8ab2b9ea6e62c.png)

**LoCon - 星瞳**

![](img/a623534fa13e26130b835e766a8aa69e.png)

**LoCon - 一些结论：**

1. 在 ghost 的例子中，我做的样本中人物尽可能大，基本会顶在图像边缘。本例子中图像边缘信息容易被丢失，导致在结果中角色不完整，我怀疑是由于卷积结构的原因（但还不能确定）。
2. 在 0.75 倍权重情况下，也会出现糊图的情况，说明 locon 虽然学习能力更强，但是泛化性略差。这一点在 ghost 和星瞳的例子上都的已验证

**LoHa - Ghost**

![](img/3bd430f460a8292e1fb40f9ce096aced.png)

**LoHa - 星瞳**

![](img/f2acb21a35019c351fb3a73d4f5a21aa.png)

**LoHa - 一些结论：**

1. 图片灰蒙蒙，不清晰（`step=40`），这一点在 ghost 和星瞳的例子上都可以反映出来，但调低`dim`参数能够一定程度上缓解。（推测：根据 https://github.com/KohakuBlueleaf/LyCORIS#lora-with-hadamard-product-representation-loha ，LoHa 的`rank<=dim^2`，所以应该用更小的`dim`参数）
2. loha 更占内存，我在跑的过程中经常爆内存
3. 听说 LoHa 在多 concept 方面表现比较好（这一点我自己暂时没去验证）
4. 总体来看，这两个 LoHa 都没有取得好的效果。训练后期会过拟合，前期又没学到东西，整体画面还不清晰。

【[补充](https://zhuanlan.zhihu.com/p/618758020)】：关于 LoHa 的训练技巧，如果读者小伙伴有更多的经验，欢迎在评论区指点一二。这里我就先贴一个关于几种模型调参数的经验贴：《Stable Diffusion Lora locon loha 训练参数设置》

#### 2.4.2.3 与 LoRA 的对比

为了统一度量，我们依然在 ghost 和星瞳两个例子上来观察效果。首先看训练过程的 loss

![](img/207f05ebd6428d8d216f843cca358d94.png)

在这两个例子中，单纯从训练过程的 loss 来看模型效果（其他参数一样），`loha > locon > lora`。但是 loss 不代表一切，从前面的例子中可以看出，locon 和 loha 分别有一些的问题，反而 loha 的效果最不理想。（这不意味着该缺点无法避免，C 站上仍然有很多优秀的 LyCORIS 模型，这里只是阐述 LyCORIS 的训练难度和使用难度可能比 LoRA 更大一些）。

## 2.5 Textual Inversion

传送门：[**论文**](https://arxiv.org/abs/2208.01618) | [**代码**](https://github.com/rinongal/textual_inversion) | [**插件**](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion)

### 2.5.1 是做什么的？

> Using only 3-5 images of a user-provided concept, like an object or a style, we learn to represent it through new “words” in the embedding space of a frozen text-to-image model.

> 只使用用户提供的概念（如对象或样式）的 3-5 张图像，我们学习通过冻结的文本到图像模型的嵌入空间中的新“单词”来表示它

其实就是学习一个新的词表示，作者在文中论证了，单纯学习词的表征已经能够学习到独特而多样的概念。

### 2.5.2 特点是什么？

1. 很少的样本量需求（论文中说 3-5 张图片）
2. 主要学习新的词和概念
3. 只更新 word embedding 的参数
4. 词 embedding 结果在微调模型上表现好，但很难迁移到其他的模型上

### 2.5.3 怎么做到的？

其实原理上很简单，从图中可以很明显看出来，作者把整个 Generator 部分都冻结，以及整个 Text Encoder 中的 Transformer 也冻结，其他词的 embedding 也都冻结，最后只留下目标词的 embedding 会参与到梯度更新中。

![](img/39695e137b1dabbca741be959962d2a8.png)

### 2.5.4 浅浅看一下代码

虽然具体使用推荐直接用 WebUI 进行训练，就已经很方便了。但是为了更加直观的理解他具体做了什么，我们还是看一下[`sd-scripts`](https://github.com/kohya-ss/sd-scripts)项目中的关键代码：

在`sd-scripts/train_textual_inversion.py`脚本中可以看到训练过程代码，整体流程上与前文的 LoRA 训练是差不多的，但是有几点关键不同：

1. 没有 create model 的过程，因为不需要创建 lora，直接在原模型基础上进行梯度更新
2. 需要对新的词进行初始化，并读取 embedding

![](img/c837a0bc6dce60e0c2935dcb40b1d991.png)

3. 需要指定出了词 embedding 以外的结构，不进行梯度更新

![](img/eb5c935a459e6e67f38f40f62a1ecfc1.png)

因此可以看出，textual_inversion 的训练，实际上是更加纯粹的一种方式。仅仅是通过参数冻结的方式，更新特定词 embedding 来进行训练。

### 2.5.5 怎么使用？

一般来说，textual_inversion 这种训练方式，由于指更新词 embedding，梯度作用范围低，限制了训练效果的上限。所以这种方法对样本的要求更高、训练难度更大，通常不是首选方法。因此本文就给出一些教程链接，和简单的样例：

+ Textual-Inversion 插件详细介绍
+ 教程 1：https://www.bilibili.com/video/BV1pm4y1A7rn
+ 教程 2：https://www.bilibili.com/read/cv19040576

为了方便对比，我们同样用 Ghost 的例子，来训练 textual_inversion。期间创建词 embedding、设置参数、开始训练等步骤已经在教程中有介绍，本文就不赘述了。我们直接看一下训练效果：

![](img/041252c03ae818a150385a3fa7285097.png)

可见虽然最终效果不见得有 Dreambooth 那样精细，但是模型已经完全能够知道"Ghost"这个词的含义了。

## 2.6 HyperNetworks

### 2.6.1 HyperNetworks 是什么

与 Textual Inversion 让梯度仅作用于词 embedding 相比，HyperNetworks 是让梯度作用于模型的 Diffusion 过程。他在 Diffusion 过程中的每一步都通过一个额外的小网络来调整去噪过程的结果。

准确的说，是作用在 Diffusion 的 Attention 部分，通过额外的小网络，把 key 和 query 进行变换，从而影响整个 diffusion 过程。

![](img/50c7ad4191fa212289efde4def148745.png)

但是和 Textual Inversion 一样，由于其效果和训练难度的原因（需要设置网络结构、训练参数等），目前并没有成为主流选择。

### 2.6.2 怎么使用？

HyperNetworks 的训练与前文介绍的 Textual Inversion 非常相似，因此关于如何使用的问题，本文不过多赘述，仅贴出相关的教程供大家参考：
