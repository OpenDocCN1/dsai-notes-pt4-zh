
# 扩散模型(Diffusion Model)基本原理及代码讲解

> 来源：[https://v11enp9ok1h.feishu.cn/docx/CqbQdyFnKoyLsvxMHKccPpUenOf](https://v11enp9ok1h.feishu.cn/docx/CqbQdyFnKoyLsvxMHKccPpUenOf)

## 推荐 PC 端打开

### 手机端浏览体验没 PC 端体验效果好

### 点击蓝字立即跳转：[🧭课程学习](https://v11enp9ok1h.feishu.cn/wiki/M1MSwcFSwiYcR0kmnt0c4M3tngd#VVFPdC1Z2oOGG5x96Jic30psnje) [知识库预览](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf) [技术指导](https://v11enp9ok1h.feishu.cn/wiki/KiIvwdFOciiqqNkwKzTcmn88ndL#Uc4XdGXLdoCWMvxxAzZcweM5nhf)

## 基本原理

扩散模型包括两个步骤：

1. 固定的（或`预设的`）前向扩散过程`q`：该过程会逐渐将高斯噪声添加到图像中，直到最终得到纯噪声。
1. `可训练`的反向去噪扩散过程：训练一个神经网络，从纯噪音开始逐渐去噪，直到得到一个真实图像。

![](img/d395fe4c5c5451bca36c1365178a2ce8.png)

前向与后向的步数由下标 `t` 定义，并且有预先定义好的总步数 `T`（`DDPM`原文中为1000）。

`t=0` 时为从数据集中采样得到的一张真实图片，`t=T` 时近似为一张纯粹的噪声。

### 直观理解

> 为了看懂扩散模型查了很多资料，但是要么就是大量的数学公式，一行行公式推完了还是不知道它想干啥。要么就是高视角，上来就和能量模型，`VAE`放一块儿对比说共同点和不同点，看完还是云里雾里。然而事实上下面几句话就能把扩散模型说明白了

> 扩散模型的目的是什么？

> 学习从纯噪声生成图片的方法

> 扩散模型是怎么做的？

> 训练一个`U-Net`，接受一系列加了噪声的图片，学习预测所加的噪声

> 前向过程在干啥？

> 逐步向真实图片添加噪声最终得到一个纯噪声

### 数学形式

#### 前向过程

`q(x_{0})`是真实数据分布（也就是真实的大量图片），从这个分布中采样即可得到一张真实图。我们定义前向扩散过程为，即每一个`step`向图片添加噪声的过程，并定义好一系列`0 < β₁ < β₂ < ... < β_T < 1`，则有：

其中，`N`为正态分布，均值和方差分别为，因此通过采样标准正态分布，有：

![](img/1d10425a3b0f9fccae48d93a6a541160.png)

![](img/201297458a75ab0e767d991573999158.png)

![](img/373d0028535ab5d501ccaccadb15c243.png)

#### 反向过程

那么问题的核心就是如何得到 的逆过程，这个过程无法直接求出来，所以我们使用神经网络去拟合这一分布。我们使用一个具有参数的神经网络去计算。假设反向的条件概率分布也是高斯分布，且高斯分布实际上只有两个参数：均值和方差，那么神经网络需要计算的实际上是

在`DDPM`中，方差被固定，网络只学习均值。而之后的改进模型中，方差也可由网络学习得到

"First,we set `\Sigma_\theta(\mathbf{x}_t,t)=\sigma_t^2\mathbf{I}` to untrained time dependent constants. Experimentally, both `\sigma_t^2=\beta_t` and `\sigma_t^2=\tilde{\beta}_t` (see paper) had similar results."

#### 模型

`Unet`中上采样和下采样模块都基于`resblock`，同时还有对`step`进行`embedding`的全连接层。数据进行下采样之后，再使上采样输出与`step embedding`向量进行相加，再输入进下一层上采样层中。

```python
class ResidualConvBlock(nn.Module):
    def __init__(
        self, in_channels: int, out_channels: int, is_res: bool = False
    ) -> None:
        super().__init__()
        '''
        standard ResNet style convolutional block
        '''
        self.same_channels = in_channels==out_channels
        self.is_res = is_res
        self.conv1 = nn.Sequential(
            nn.Conv2d(in_channels, out_channels, 3, 1, 1),
            nn.BatchNorm2d(out_channels),
            nn.GELU(),
        )
        self.conv2 = nn.Sequential(
            nn.Conv2d(out_channels, out_channels, 3, 1, 1),
            nn.BatchNorm2d(out_channels),
            nn.GELU(),
        )

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        if self.is_res:
            x1 = self.conv1(x)
            x2 = self.conv2(x1)
            # this adds on correct residual in case channels have increased
            if self.same_channels:
                out = x + x2
            else:
                out = x1 + x2
            return out / 1.414
        else:
            x1 = self.conv1(x)
            x2 = self.conv2(x1)
            return x2


class UnetDown(nn.Module):
    def __init__(self, in_channels, out_channels):
        super(UnetDown, self).__init__()
        '''
        process and downscale the image feature maps
        '''
        layers = [ResidualConvBlock(in_channels, out_channels), nn.MaxPool2d(2)]
        self.model = nn.Sequential(*layers)

    def forward(self, x):
        return self.model(x)


class UnetUp(nn.Module):
    def __init__(self, in_channels, out_channels):
        super(UnetUp, self).__init__()
        '''
        process and upscale the image feature maps
        '''
        layers = [
            nn.ConvTranspose2d(in_channels, out_channels, 2, 2),
            ResidualConvBlock(out_channels, out_channels),
            ResidualConvBlock(out_channels, out_channels),
        ]
        self.model = nn.Sequential(*layers)

    def forward(self, x, skip):
        x = torch.cat((x, skip), 1)
        x = self.model(x)
        return x


class EmbedFC(nn.Module):
    def __init__(self, input_dim, emb_dim):

```

### 网络训练流程

我们最终要训练的实际上是一个噪声预测器。神经网络输出的噪声是，而真实的噪声取自于正态分布。则损失函数为：

预测网络方面，`DDPM`采用了 `U-Net`。

![](img/f521d4c9b6da1dc4a5f9649730ec4630.png)

从而，网络的训练流程为：

1. 我们接受一个随机的样本；
1. 我们随机从 `1` 到 `T` 采样一个 `t`；
1. 我们从高斯分布采样一些噪声并且施加在输入上；
1. 网络从被影响过后的噪声图片学习其被施加了的噪声。

## Diffusion流程

### 运行流程

`Diffusion`分为两个步骤——扩散、逆扩散

![](img/6b0ee0e1655902589eaacf9813cdde87.png)

扩散过程是对图像加入高斯噪声的过程（图中上半部分）：

给定一张图像，然后构造`T`个时刻，每一个时刻对应一张图像，如图中`t=0`，对应我们给定的初始图像

然后，对这张图像加一个高斯噪声，得到`t=1`时刻的图像；再对`t=1`时刻的图像加入噪声，得到`t=2`时刻的噪声。然后重复此法，到`T`时刻时，就得到了一张全是噪点的图像（`t=T`）

逆扩散过程就是对图像减去噪声的过程，也就是还原图像的过程（图中下半部分）：

给定一张全是噪点的图像，然后不断去噪，去到`t=2`时，得到图中的图像，再去噪，得到`t=1`时刻的图像，再再去噪，就还原回了坤坤的图像。

### 原因

为什么要费尽心思把它加入这么多噪声，再复原回来？

对于`T`时刻的图像，它是服从正态分布的，并且，是近似服从标准正态分布。那么，如果要生成图像，是不是可以直接从标准正态分布中采样出数据，然后一点点去噪，就能还原回原来的图像了呢？这就是`diffusion`的思想

### 前向加噪

记时刻1为原始图像，表示为`x₁`，则时刻2表达为`x₂`

每一个时刻都要加入一个噪声，那么该如何加噪呢？假设在`t−1`时刻，我们该如何得到`i`时刻的图像？其公式如下

`x_{t-1}` 和 `x_t` 分别表示`t−1`时刻和`t`时刻的图像。`β_t` 一般是一个大于0小于1的超参数，随着时刻的增加而减小，`ε_t ~ N(0, I)`的标准正态分布（Ps：为什么是加权平均，并且要开根号？请看参考①）

+ 从这个式子不难看出，这其实不是简单的从 `x_{t-1}` 中加噪，而是加权平均。

从`t−1`到`t`可以用式（1）表示，那从`t−2`到`t`呢？难道我们每次都要一次一次的加入噪声吗？有没有办法更好的表示？

我们先看一个正态分布的基本定理：

> 定理①：

> 假设：

> 则：

> 定理②：

> 假设：

> 则：

> Ps：这几个定理证明很简单，此处不做证明，不懂的可以自行百度，或者问我。

现在，我们把 `x_0` 到 `x_T` 由式（1）推导出来

### 逆扩散过程

> 得到了 `x_T` 的加噪过程及其概率分布。前面提到，我们的最终目标是从`T`时刻采样出数据，然后慢慢去噪，就得到生成的图像。那么，该如何去噪呢？换句话说，我们该如何求出 `x_{t-1}` 呢？

### 一阶马尔可夫假设

在扩散过程中，模型总是一个时刻一个时刻地加噪，也就是说，`t`时刻的图像，是来自`t−1`时刻的

![](img/d8178f84ac6c608a889de15c1acad87a.png)

所以，在这种过程中，引入一个假设——马尔可夫假设

一阶马尔可夫假设：给定`t−1`时刻的状态，`t`时刻仅与`t−1`时刻有关，与过去的状态无关

概率表达为：

逆扩散也是一样的道理（按照图中箭头即可看到）

由马尔可夫假设，我们不难得出：

然后不断拆分，就得到了：

## Diffusion文字生成图片——整体结构

### 整个生成过程

![](img/df225cb59528c3f276f9798732c11a4d.png)

▲ 文字生成图片全过程

实际上 `Diffusion` 是使用 `Text Encoder` 生成文字对应的 `embedding`（`Text Encoder` 使用 `CLIP` 模型），然后和随机噪声 `embedding`，`time step embedding` 一起作为 `Diffusion` 的输入，最后生成理想的图片。我们看一下完整图：

![](img/fbd3f645cf03e99888582102047670db.png)

▲ `token embedding`、随机噪声`embedding`、`time embedding`一起输入`diffusion`

上图我们看到了 `Diffusion` 的输入为 `token embedding` 和随机 `embedding`，`time embedding` 没有画出来。中间的 `Image Information Creator` 是由多个 `UNet` 模型组成，更详细的图如下：

![](img/8ee5c78fec2acfdff5999af7bfd725e6.png)

▲ 更详细的结构

可以看到中间的 `Image Information Creator` 是由多个 `UNet` 组合而成的，关于 `UNet` 的结构我们放在后面来讲。现在我们了解了加入文字 `embedding` 后 `Diffusion` 的结构，那么文字的 `embedding` 是如何生成的？接下来我们介绍下如何使用 `CLIP` 模型生成文字 `embedding`。

### 使用CLIP模型生成输入文字embedding

> `CLIP` 在图像及其描述的数据集上进行训练。想象一个看起来像这样的数据集，包含 4 亿张图片及其说明：

![](img/ee8fe22a635e7e40740449b4b47dc402.png)

▲ 图像及其文字说明

> 实际上 `CLIP` 是根据从网络上抓取的图像及其文字说明进行训练的。`CLIP` 是图像编码器和文本编码器的组合，它的训练过程可以简化为给图片加上文字说明。首先分别使用图像和文本编码器对它们进行编码。

![](img/3e8ba20ec9e95c472f6ba709dd496d0b.png)

> 然后使用余弦相似度刻画是否匹配。最开始训练时，相似度会很低。

![](img/9c680d2bc85db604af9289a15b10042f.png)

> 然后计算 `loss`，更新模型参数，得到新的图片 `embedding` 和文字 `embedding`。

![](img/e85d09791673f60338e845e974ea0762.png)

### UNet网络中如何使用文字embedding

前面已经介绍了如何生成输入文字 `embedding`，那么 `UNet` 网络又是如何使用的？实际上是在 `UNet` 的每个 `ResNet` 之间添加一个 `Attention`，而 `Attention` 一端的输入便是文字 `embedding`。如下图所示：

![](img/d31ec296d65f1936d96ae3d19e08dd29.png)

更详细的图如下：

![](img/943281f891105e89e0383e3c5de30325.png)

## 扩散模型Diffusion

前面介绍了 `Diffusion` 是如何根据输入文字生成图片的，让大家有个大概的了解，接下来会详细介绍扩散模型 `Diffusion` 是如何训练的，又是如何生成图片的。

### 扩散模型Duffison的训练过程

![](img/3b41a5d8f99d85243e4b2aef1d302319.png)

▲ 扩散模型`Diffusion`

> `Diffusion` 模型的训练可以分为两个部分：

1. `前向扩散过程（Forward Diffusion Process）`→图片中添加噪声；
1. `反向扩散过程（Reverse Diffusion Process）`→去除图片中的噪声。

#### 前向扩散过程

![](img/9ce8f0a953c380f77ba50274411b6902.png)

前向扩散过程是不断往输入图片中添加高斯噪声。

#### 反向扩散过程

![](img/811abb5833c003a4559cf5850463a2ab.png)

反向扩散过程是将噪声不断还原为原始图片。

#### 训练过程

![](img/89702f7addee09c44874677035ffe2f7.png)

在每一轮的训练过程中，包含以下内容：

1. 每一个训练样本选择一个随机时间步长 `t`。
1. 将 `time step t` 对应的高斯噪声应用到图片中。
1. 将 `time step` 转化为对应 `embedding`。

**下面是每一轮详细的训练过程：**

![](img/2f7f9793aa0c638fe00d503560071ef4.png)
