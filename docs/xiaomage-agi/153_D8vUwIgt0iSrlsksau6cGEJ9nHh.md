# SORA

> 来源：[https://xmgtoagi.feishu.cn/docx/Mjr9daytoo2d8zxReghcASNsnHc](https://xmgtoagi.feishu.cn/docx/Mjr9daytoo2d8zxReghcASNsnHc)

> 译者：小码哥

> 原文地址：https://openai.com/research/video-generation-models-as-world-simulators

![](img/790e61ea79098a3b86e490754fadabe7.png)

[title_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HweobTHHgotfk7xDjOpcDtT3nyf)

我们探索视频数据生成模型的大规模训练。具体来说，我们在可变持续时间、分辨率和宽高比的视频和图像上联合训练文本条件扩散模型。我们利用对视频和图像潜在代码的时空补丁进行操作的变压器架构。我们最大的模型 Sora 能够生成一分钟的高保真视频。我们的结果表明，扩展视频生成模型是构建物理世界通用模拟器的一条有前途的途径。

本技术报告重点关注（1）我们将所有类型的视觉数据转化为统一表示的方法，从而能够大规模训练生成模型，以及（2）对 Sora 的能力和局限性进行定性评估。本报告不包含模型和实施细节。

许多先前的工作已经研究了使用各种方法对视频数据进行生成建模，包括循环网络、(1 ,)(2 ,)(3)生成对抗网络，(4 ,)(5 ,)(6 ,)(7)自回归变压器，(8 ,)(9)和扩散模型。(10 ,)(11、)(12)这些作品通常关注一小类视觉数据、较短的视频或固定大小的视频。 Sora 是视觉数据的通用模型，它可以生成不同时长、长宽比和分辨率的视频和图像，最多可达一分钟的高清视频。

## 将视觉数据转化为补丁

我们从大型语言模型中获得灵感，这些模型通过互联网规模数据的训练来获得通用能力。(13、)(14)LLM 范式的成功部分归功于令牌的使用， 这些令牌优雅地统一了文本代码、数学和各种自然语言的不同模式。在这项工作中，我们考虑视觉数据的生成模型如何继承这些好处。 LLM 有文本标记，而 Sora 有视觉补丁。此前，补丁已被证明是视觉数据模型的有效表示。(15 ,)(16 ,)(17、)(18)我们发现补丁是一种高度可扩展且有效的表示形式，可用于在不同类型的视频和图像上训练生成模型。

![](img/f04dfccf184ddc66b518c60518c96e70.png)

在较高的层次上，我们首先将视频压缩到较低维的潜在空间，将视频转换为补丁，(19)然后将表示分解为时空补丁。

## 视频压缩网络

我们训练一个降低视觉数据维度的网络。(20)该网络将原始视频作为输入并输出在时间和空间上压缩的潜在表示。 Sora 在这个压缩的潜在空间中接受训练并随后生成视频。我们还训练了相应的解码器模型，将生成的潜伏映射回像素空间。

## 时空潜伏斑块

给定一个压缩的输入视频，我们提取一系列时空补丁，充当变压器令牌。该方案也适用于图像，因为图像只是具有单帧的视频。我们基于补丁的表示使 Sora 能够对不同分辨率、持续时间和长宽比的视频和图像进行训练。在推理时，我们可以通过在适当大小的网格中排列随机初始化的补丁来控制生成视频的大小。

## 用于视频生成的缩放变压器

Sora是一个扩散模型(21、)(22、)(23、)(24、)(25);给定输入噪声补丁（以及文本提示等调节信息），它被训练来预测原始的“干净”补丁。重要的是，Sora 是一个扩散变压器。(26)Transformer 在各个领域都表现出了卓越的扩展特性，包括语言建模、(13、)(14)计算机视觉，(15 ,)(16 ,)(17、)(18)和图像生成。(27、)(28 ,)(29)

![](img/4034cd201e515332634084e7a6b7726a.png)

这项工作中，我们发现扩散变压器也可以有效地缩放为视频模型。下面，我们展示了训练过程中具有固定种子和输入的视频样本的比较。随着训练计算的增加，样本质量显着提高。

![](img/a33d16ae71ccdda743f9bbca47f52a56.png)

[scaling_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LkHFb3Zfso1gW1xU503csUJhnUh)

![](img/22a8adf86f7858e50a03ed2949eaf457.png)

[scaling_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ErURbR7E9okI4Mx1LFHcnvdJnQc)

![](img/ebccff9df86da79e644569974d94d888.png)

[scaling_2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RFfRbCNroo04YOxO4yaclHepnze)

# 可变的持续时间、分辨率、宽高比

过去的图像和视频生成方法通常会将视频调整大小、裁剪或修剪为标准尺寸，例如，分辨率为 256x256 的 4 秒视频。我们发现，对原始大小的数据进行训练有几个好处。

采样灵活性

Sora 可以采样宽屏 1920x1080p 视频、垂直 1080x1920 视频以及介于两者之间的所有视频。这使得 Sora 可以直接以其原生宽高比为不同设备创建内容。它还使我们能够在以全分辨率生成之前快速以较低尺寸制作原型内容 - 所有这些都使用相同的模型。

![](img/524f4b9cfab38141df9cb1a6bb134521.png)

[sampling_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BE6PbV9PkoiDnHxw4X2cRFLHnCe)

![](img/a526266f523f729ce03ae615d9fe294e.png)

[sampling_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ex0NbWQmnoPiXJxUudwcZMlJnVh)

![](img/4e310fe1960f06bbfe1682892cbaee3e.png)

[sampling_2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WCFBbfnOAoQ50yxRF5dcaBw2nDe)

# 改进的框架和构图

我们根据经验发现，以原始长宽比对视频进行训练可以改善构图和取景。我们将 Sora 与将所有训练视频裁剪为正方形的模型版本进行比较，这是训练生成模型时的常见做法。在方形作物（左）上训练的模型有时会生成仅部分可见主体的视频。相比之下，Sora（右）的视频的取景效果有所改善。

![](img/ca3c32de58e84c3fba368cd470762677.png)

[sampling_3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NirwbFA8soJ11jxkEBFcEmxYnee)

![](img/6bef480530ab834cc758f61e83bda29b.png)

[sampling_4.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QkeqbXo5wocUYDxWfbbcyyt9nmb)

# 语言理解

训练文本到视频生成系统需要大量带有相应文本字幕的视频。我们应用了 DALL·E 3 中引入的重新字幕技术30到视频。我们首先训练一个高度描述性的字幕生成器模型，然后使用它为训练集中的所有视频生成文本字幕。我们发现，对高度描述性视频字幕进行训练可以提高文本保真度以及视频的整体质量。

与 DALL·E 3 类似，我们还利用 GPT 将简短的用户提示转换为较长的详细字幕，然后发送到视频模型。这使得 Sora 能够生成准确遵循用户提示的高质量视频。

# 通过图像和视频进行提示

上面和我们的着陆页中的所有结果都显示文本到视频的示例。但 Sora 也可以通过其他输入进行提示，例如预先存在的图像或视频。此功能使 Sora 能够执行各种图像和视频编辑任务 - 创建完美的循环视频、动画静态图像、及时向前或向后扩展视频等。

DALL·E 图像动画

Sora 能够生成提供图像和提示作为输入的视频。下面我们展示基于DALL·E 2生成的示例视频31和达尔·E 330图片。

![](img/bfc3790a0df2cc1c1224f004eb65fa21.png)

![](img/f4d10e28a74338c316c9822a0f24744f.png)

[prompting_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XRp7bO6xIopVh1xFZoQcfSK8nMd)

一只戴着贝雷帽和黑色高领毛衣的柴犬。

![](img/ffe12e0ce3996ebb3da89bf6ce8db38e.png)

![](img/083727c4cd70a35e950275628cf9ef39.png)

[prompting_3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UU2ibkdcao5sYOxiktJcim2lnGh)

不同家族怪物的平面设计风格的怪物插图。该群体包括一个毛茸茸的棕色怪物、一个带有天线的光滑黑色怪物、一个有斑点的绿色怪物和一个小圆点怪物，所有怪物都在一个有趣的环境中互动。

![](img/7cdce80b1b950f81e4625c23138cf3d2.png)

![](img/95b434cb07fa8fcdd210fc09412dfd19.png)

[prompting_5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DW5ubBAHMolPjFxcb1GchRJEnkg)

写有“SORA”的现实云的图像。

![](img/15c3f2da982e861bc266e836e61d2f0f.png)

![](img/085b71c73259821a0b908cfa00b5ed1b.png)

[prompting_7.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Gi3KbMaIzoScAYxmhoZc4csCnLe)

在一座华丽的历史大厅里，巨大的浪潮达到顶峰并开始崩塌。两名冲浪者抓住时机，熟练地驾驭海浪。

# 连接视频

我们还可以使用 Sora 在两个输入视频之间逐渐进行插值，从而在具有完全不同主题和场景构成的视频之间创建无缝过渡。在下面的示例中，中心的视频插值在左侧和右侧的相应视频之间。

![](img/1d2f8273aedea261687b26936b8a5ab4.png)

[d1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BOUob9o1OoBvjYxRSBQcuRdlnNd)

![](img/48ba3c95d484c021037bb7cb9db8664f.png)

[a0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UD01bLcBfoULNYxRnkCcub7DnOe)

![](img/1e9355c407786323f2e86a7f502725a8.png)

[a1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LjNbbiEUMoxANUxRtvFcA48Jndb)

![](img/df0f11c294d82aedd1628422d54907c4.png)

[a2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ImPmbPMpko5EuZxUDZkcXT8knQc)

![](img/eeddaa8ffa3de6c584591cbbc4b1ea73.png)

[b1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OwkVbetP0oTbrMxcAyBc1xmcnXd)

![](img/45fd4f1dfffc2f3098ba29c28de8cce2.png)

[b2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TQgHbTV0AoFs1Txrzt8cHGLWnig)

![](img/ff0bc70e7aa82406dac4c0ecb8192e6b.png)

[bike_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NjLXbeUFxovySCxXguKcsF9Onqe)

![](img/16a73d2f418b7f9b78486fa4a3bae58e.png)

[c0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TUjJbLHpooD7brxOnL4cj1Kgnxd)

![](img/0ae91437c0647010271cb9cf73dc5e16.png)

[c1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VSb1bYPiIoh4L1xoddxcS00Nncd)

![](img/4e0b1b2952c038881318e17d10427f23.png)

[c2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CgiGbZKodoHmHZx4mjpckOqtnwc)

![](img/6b3899d7fc6e377367f044f7371dac3e.png)

[d0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TAhEbAsW7oL4kAxGic6cptCin4f)

![](img/33a297b5c35f2067ef6bb05b915732bd.png)

[d0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AsesbgTdFof3fbxfjQIcQqhfnhg)

![](img/08acaf8c97de8de064c6b20fafa99c70.png)

[simulation_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SszqbDyJlofff9xx3gWcpfiSnXg)

![](img/215deefd0893f5cd704b33ef224f221e.png)

[simulation_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WcTobSer1oEQRyxVhzsc2ZEenkh)

![](img/93f3885b3c75ee6ebb804eaf516e3d0a.png)

[d1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Fs33bFurdoHAjVxVbUecItTvnnk)

![](img/c9c06efcbf4ffbac9926c7b98ed753af.png)

[d2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FQhxb4BcGo9V2uxgEypcCKzGnTd)

![](img/3755ff2733c98b33fadd327da1a9852c.png)

[e0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F7jHbTiQYoicJjxHqy0cmobgnUU)

![](img/0b29550837981cf47eae17115cfbe663.png)

[e1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FcvlbTfuroRKkJx1zVqcRMlNnhf)

![](img/45368672a425d38876f9b0f8882810c3.png)

[e2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YHQ8bGCmlo69Mmxvmraci9kwn4e)

# 图像生成能力

Sora 还能够生成图像。我们通过在时间范围为一帧的空间网格中排列高斯噪声块来实现这一点。该模型可以生成各种尺寸的图像，分辨率高达 2048x2048。

![](img/98c8f93e80329099173ef20f2a37b4fd.png)

![](img/53ffd34f54d783ea33dc737c04cff527.png)

秋季女性特写肖像照，极其细节，浅景深

![](img/026735e354f862a48c692d036edaa522.png)

![](img/012196a90f197687d4c4885bce689f8b.png)

充满活力的珊瑚礁，充满色彩缤纷的鱼类和海洋生物

苹果树下一只小老虎的数字艺术，采用哑光绘画风格，细节华丽

雪山村庄，拥有舒适的小屋和北极光显示屏，高细节和逼真的数码单反相机，50 毫米 f/1.2

新兴的模拟功能

我们发现，视频模型在大规模训练时表现出许多有趣的新兴功能。这些功能使 Sora 能够模拟现实世界中人、动物和环境的某些方面。这些属性的出现对 3D、物体等没有任何明确的归纳偏差——它们纯粹是尺度现象。

**3D 一致性。** Sora 可以生成带有动态摄像机运动的视频。随着摄像机的移动和旋转，人和场景元素在三维空间中一致移动。

![](img/2967d68db791ea15f57added3e205954.png)

[simulation_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KWArbwx5JoUyO8xZEBucHgyBnHg)

![](img/225c8aee54cb40d75c33dd9fcb2c7443.png)

[simulation_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UlXubQJbOowoLZxLOOYcXF6WnKh)

**远程相干性和物体持久性。**视频生成系统面临的一个重大挑战是在采样长视频时保持时间一致性。我们发现 Sora 通常（尽管并非总是）能够有效地对短期和长期依赖关系进行建模。例如，我们的模型可以保留人、动物和物体，即使它们被遮挡或离开框架。同样，它可以在单个样本中生成同一角色的多个镜头，并在整个视频中保持其外观。

![](img/dd5f27ec3e74a14f086033bf90e99850.png)

[simulation_3.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EG6Xb6efAoNVX9xT8NrcX3j6nMh)

![](img/56f863e4f4fda3c73030b560d753f47d.png)

[simulation_2.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DFkTbNTLto7wCUxG2EtcuprSnld)

**与世界互动。**索拉有时可以用简单的方式模拟影响世界状况的行动。例如，画家可以在画布上留下新的笔触，并随着时间的推移而持续存在，或者一个人可以吃汉堡并留下咬痕。

![](img/cc9cad8f665d4bb63c6025b489cc35fd.png)

[simulation_5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RUTYbrIZMoYrljx0gl8ct779nQG)

**模拟数字世界**。 Sora 还能够模拟人工过程——一个例子是视频游戏。 Sora 可以同时通过基本策略控制《我的世界》中的玩家，同时以高保真度渲染世界及其动态。这些能力可以通过用提及“我的世界”的标题提示 Sora 来零射击。

![](img/a35be84e25416c3d14e45d8d0df451f3.png)

[simulation_7.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/KL0mbZvcQolgvDxJnvbcx4SanXf)

![](img/bb942b602a71da7adfb454538574de35.png)

[simulation_6.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ke8YbsuXBocsE3xXhTJc7ZMSnKb)

这些功能表明，视频模型的持续扩展是开发物理和数字世界以及生活在其中的物体、动物和人的高性能模拟器的一条有前途的道路。

# 讨论

![](img/f7b5be0aff75631b56f159c9faceb3b2.png)

[discussion_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Gudyb9e6co6qWSxEqylcHIitnWd)

Sora 目前作为模拟器表现出许多局限性。例如，它不能准确地模拟许多基本相互作用的物理过程，例如玻璃破碎。其他交互（例如吃食物）并不总是会产生对象状态的正确变化。我们在登陆页面中列举了模型的其他常见故障模式，例如长时间样本中出现的不连贯性或对象的自发出现。

![](img/dd96159b380b992832e9ecfd02f4a6be.png)

[prompting_5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HhsHbM93mojjsMxZfAHcaIdAn4f)

我们相信，Sora 今天所拥有的能力表明，视频模型的持续扩展是开发物理和数字世界以及生活在其中的物体、动物和人的强大模拟器的一条有前途的道路。