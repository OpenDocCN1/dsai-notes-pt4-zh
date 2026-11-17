
# OpenAI新的AI文生视频应用——Sora将带来什么？（信息汇总及个人思考）

> 来源：[https://agijuejin.feishu.cn/docx/BiLzdblX1ofF27x10w8ck1RAneh](https://agijuejin.feishu.cn/docx/BiLzdblX1ofF27x10w8ck1RAneh)
> 作者：德里克文
> 原文：[https://zhuanlan.zhihu.com/p/682556788](https://zhuanlan.zhihu.com/p/682556788)

`Sora`这几天的爆炸性新闻，让所有人工智能相关从业者及对应用感兴趣的人群都感到沸腾，震撼到央视也在进行相关的讨论，简直可以和2023年初ChatGPT讨论带来的热潮一般。

![](img/487fd2bf3cb3d60fa90ad377a70bfdb2.png)

我整理了下最近两天的相关内容给大家做一个回顾参考。

先看下`Sora`最新的视频效果和和其他文生视频的对比。

![](img/0620d054292566e2efffe2059b8a485f.png)

[96f12c1c-cda8-11ee-9b74-ee02f1e2e9af-v8_f2_t1_fSwcntFE.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WPh2bNrw7oiPm1x5tt3ckHDKn5b)

![](img/90327a38871e0a6f06c952f90e511025.png)

[494c9adc-cda8-11ee-8611-d2edd8d9eff0-v8_f2_t1_4oYSxhi5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NcYmb4o3DoreW9xgbPGcQk2Nni2)

[6304ddae-cda8-11ee-8485-dafec810f3f1-v8_f2_t1_linU4p5O.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K6dIb3oxzoWNxexySQgc9hV2nFh)

![](img/52071bfbc79db0d782c647c70bbb4214.png)

[bbc8a9ee-cdac-11ee-9733-c66630469733-v8_f2_t1_SKWKUlEx.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Yy3WbtQRYo7WnOxppoWcHLchnif)

## 一、什么是SORA?

`Sora` 是 OpenAI 最新发布的文本生成视频模型，不仅可以生成长达一分钟的视频，且能完全遵照用户的 Prompt 并保持视觉质量。

OpenAI 这个公司的格局非常大，他想要做 World Simulators（世界模拟器），做通用AGI，而不仅仅是文字或者图像视频领域的内容，他希望的是帮助人们解决需要现实世界交互的问题。

### 1. Sora 官网

[ ](https://link.zhihu.com/?target=https%3A//openai.com/sora)

![](img/650dfd0ebecd4eac120f8a0d87a504f1.png)

### 2. Sora 论文

OPENAI对于`Sora`的论文可以详见下列链接：

[ ](https://link.zhihu.com/?target=https%3A//openai.com/research/video-generation-models-as-world-simulators)

我挑选大众关心的重点内容翻译：

![](img/cf8bc2b575233847bc17a84ce200aed4.png)

图片中文翻译：

> 视频 [生成模型 ](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)作为世界模拟器 我们探讨了在视频数据上对生成模型进行[大规模训练。 具体](https://www.zhihu.com/search?q=%E6%96%87%E6%9C%AC%E6%9D%A1%E4%BB%B6%E6%89%A9%E6%95%A3%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)来说，我们共同训练了 文本条件扩散[模型 ，这些](https://www.zhihu.com/search?q=%E5%8F%98%E5%8E%8B%E5%99%A8%E6%9E%B6%E6%9E%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)模[型能够处理](https://www.zhihu.com/search?q=%E6%89%A9%E5%B1%95%E8%A7%86%E9%A2%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)不同时长、分辨率和宽高比的视频和图像。 我们利用了一种 变压器架构 ，该架构能够处理视频和图像潜在代码的空间时间块。我们最大的模型，`Sora`，能够生成一分钟的高保真视频。 我们的结果表明， 扩展视频 生成模型是构建通用物理世界模拟器的有希望的道路。

![](img/79554dd128b0019bb40301b2549bd4fc.png)

中文翻译：

> 语言[理解 训](https://www.zhihu.com/search?q=%E8%AE%AD%E7%BB%83%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)练文本到视频生[成系统需要](https://www.zhihu.com/search?q=%E7%94%9F%E6%88%90%E6%96%87%E6%9C%AC&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)大量的带有相应文本字幕的视频。 我们将DALL·E3中引入的重新字幕技术应用到视频中。我们首先训练一个高度描述性的字幕模型然后使用它为 训练集 中的所有视频 生成文本 字幕。 我们发现，高度描述性的视频字幕训练提高了文本保真度以及视频的整体质量。 与DALL·E3类似，我们还利用GPT将简短的用户提示转换为较长的详细字幕，并发送给视频模型。 这使得`Sora`能够准确地按照用户提示生成高质量的视频。

而关于论文内部详细的内容，网上有整理好的资料，建议可以参考这个 [飞书文档 ](https://link.zhihu.com/?target=https%3A//caaiartlab.feishu.cn/docx/LgmFdXTRwo0m5yxkQRIcO6RGnRf)，有对这个论文的翻译及详细的解析，分享给大家，不用花冤枉钱去购买。

![](img/c9fbdf4fc8def8b4f049dba520b18402.png)

### 3. Sora 怎么使用

需要强调给大家的一点是，截止至2024年2月17日，`Sora`目前没有公开测试，也没有内测申请渠道，国内是没有渠道可以去尝试的，因此大家千万不要相信现在所谓的`Sora`课程，这些人都自己没有操作过。

![](img/833ab2cb88b2978746f57a10c61dff77.png)

## 二、关于`Sora`的讨论

### 1. 各方观点

下面从互联网上收集了部分信息，供大家参考

![](img/d12e10b4f0d8000e7a3cf78868a72d9e.png)

![](img/867229b631cb963f58a721262cb0caa5.png)

### 2. Sora 会议小结

而在2月6日晚上，有一个人工智能论坛讨论关于`Sora`之后，生成视频往何处去的内容，我征得记录者同意，将晚上讨论的内容分享给大家，

![](img/f23777a34844e893423c8d40257da06d.png)

## 三、详细解读

**我认为这是AI视频领域的原子弹！“60s超长长度”、“超强语义理解”和“世界模型”每一条都足以震撼[人工智能应用](https://www.zhihu.com/search?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E5%BA%94%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)！**

对比现在文生视频的一流团队RUNWAY、PIKA、SVD的生成效果，简直是造成了跨时代的碾压震撼效果！Runway 和Pika目前生成的视频时长都较短只有几秒，需要通过不断的拼接。

![](img/00be14e46c6d955e09f02d67512fcefb.png)

[c1d376e2-cdad-11ee-909d-b6a2d25e72c2-v8_f2_t1_c84Wrl1e.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Y9vMblJbooCqt7xJwR7clrL8nFf)

而且画面稳定性不强，如果需要呈现好的效果，需要创作者本身有非常强的视频剪辑及相关基础。

而`SORA`这次最逆天的是，通过非常简单的文字描述，就可以生成画面稳定，理解能力强的 长视频 ！

`Sora`本次展示的是技术思路不同所带来的完全碾压。

从关注 二维像素 的变化，变成关注语义理解的变化，从视频画面的生成，变成故事逻辑的生成。

![](img/f9136b779f467aa876d147afd95417fb.png)

[e032162a-cdad-11ee-94fd-2ad705445e65-v8_f2_t1_n1ypP0s0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NcScbthJroNtI1xGudycPRE4n8r)

之前无论是Runway、Pika、SVD等等文生图、文生视频都是在二维平面上对图像进行调整和组合，但是`Sora`的视频，显示它能像人一样理解一些基础的物理规律，这是 OpenAI利用它的 大语言模型 优势进行的超强语义理解，是真正层面的世界模型。

**只有实现对现实世界的理解和对真实世界的模拟，**这样产生的图像和视频才是更加真实的效果。

这次`Sora`带来的震撼或许不仅仅是影视行业，而是未来可能扩展到其他行业，视频展示的是对真实世界物理规律的再现！

![](img/408c565d91d8f5160529b4b7460f4903.png)

[25301100-cdae-11ee-ba31-1abe7b4fb76d-v8_f2_t1_RYx6Q6SQ.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IkrGbTM10o8ERnxREuMcsgQin8f)

英伟达的高级科学家 [Jim Fan](https://www.zhihu.com/search?q=%E8%99%9A%E5%B9%BB%205&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D) 认为 `Sora` 的实现原理，这不仅仅是一个视频生成模型这么简单，还是一个 基于数据驱动的 [虚幻 5](https://www.zhihu.com/search?q=%E8%99%9A%E5%B9%BB%205&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D) 引擎 。

可以把 `Sora` 看作是一种可学习的模拟器，或者说是一个能模拟现实世界的“世界模型”。

这种方法可以让 `Sora` 更好地理解和模拟现实世界的物理现象。

![](img/7c38b0923e62317a7f21bb220d60383e.png)

[2f4c4f28-cdae-11ee-acc1-dab8c54acdb4-v8_f2_t1_SGExcL8m.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SYd9b5hEFoztu6xDCsachDcFnng)

### 1. 世界模拟器

OpenAI目前开发的`Sora`视频生成模型技术，将完全超越现有的视频生成模型，如Runway和Pika。

这项技术的核心是一个创新的“世界模拟器”，它是一个基于文本条件的扩散模型，通过从大量的视频中学习，这些视频涵盖了不同的时长、宽高比和分辨率。

这个模拟器的训练过程涉及吸收和处理海量的视觉数据，使其能够根据文本描述生成相应的视频内容。

例如，当输入“太空人的冒险故事，他戴着一顶红色羊毛编织的 摩托车头盔 ”这样的描述时，模型能够理解含义，并且生成与之相符的视频画面。

![](img/afee43d2d8393783fec095c9c4419e04.png)

该模型还具备生成视频的灵活性和多样性，支持不同的时长和分辨率设置，其最大输出规格可达1920*1080的分辨率和30帧/秒的帧率。

### 2. 视觉数据转换

简单来说，OpenAI在 视觉数据处理 领域，将视觉数据转换为“patch”这一个个单元体，它可以将图像和视频帧分割成“补丁”状的小块。



这些“补丁”作为视觉模型的基本输入单元，使得模型能够学习和理解如何表示以及重建视觉场景。在此基础上，模型能够在特定条件，如文本描述的引导下，生成新的图像或视频内容。

![](img/77ec2bdffad717336bcdf06b1df37ab9.png)

这种处理方式与 [大型语言模型 ](https://www.zhihu.com/search?q=%E5%A4%A7%E5%9E%8B%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)中的“token”概念相似，token是文本数据的基本处理单元。

在[语言模型中，](https://www.zhihu.com/search?q=patch&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)文本被分解为较小的片段以实现语言的理解和生成。同样地，视觉模型的训练过程涉及将不同类型的视频和图片转换成 patch ，作为模型输入的基本单位。

[这个过程可](https://www.zhihu.com/search?q=%E8%A7%86%E9%A2%91%E8%BD%AC%E6%8D%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)[以理解成首](https://www.zhihu.com/search?q=%E6%97%B6%E7%A9%BA%E8%A1%A5%E4%B8%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)先将视频压缩到一个较低维的潜在空间，然后将 视频转换 为patch，并进一步分解为“spacetime patches”( 时空补丁 )。

### 3、视频压缩

研究者开发出一种专门的视频压缩网络。

该网络的核心是一个经过训练的神经网络，其设计宗旨在于降低视觉数据的多维度复杂性。

而所谓的“降低维度”，指的是将数据从高维空间——例如原始视频数据，包含了海量的像素信息——转换到低维空间。这一过程的目的是对数据进行简化，提取关键特征，同时减少后续处理所需的计算资源。

![](img/07b09be702bea5d2cb9aee68ebcd7c16.png)

这个神经网络接受原始视频作为输入，并输出一个在时间和空间上都经过压缩的潜在表示（latent representation）。

时间上的压缩意味着减少了表示视频动态变化所需的信息量；

空间上的压缩则意味着减少了表示视频中每一帧图像所需的信息量。

在这个压缩的潜在空间中， [Sora模型 ](https://www.zhihu.com/search?q=Sora%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)首先进行训练，学习如何理解和控制这种形式的数据。经过训练，Sora能够在这个潜在空间内生成新的视频数据。

![](img/ad81d71857b31b6cb68586d773fe4741.png)

为了将Sora生成的潜在表示转换回原始的像素空间，研究者还训练了一个解码器模型。

[解码器的作](https://www.zhihu.com/search?q=%E8%A7%86%E9%A2%91%E6%A0%BC%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)用是将压缩的视频数据还原成可以直接观看的 视频格式 。

### 4、时空补丁（Spacetime Laten Patches）

在视频数据压缩完成后，接下来的关键步骤是提取一系列的“Spacetime Latent Patches”，这些Patches包含了视频在特定时间和空间范围内的信息。

[这些Patch](https://www.zhihu.com/search?q=%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)es在transformer模型中扮演的角色类似于 自然语言处理 中的单词token。

这种方法不仅适用于视频数据，也适用于图形数据，使得不同分辨率、时间和宽高比的视频和图像能够作为Sora模型的训练集。

![](img/fad3438fbe5948614b62695c91401ffb.png)

在模型推理，即生成新的视频内容时，可以通过在适当大小的网格中排列随机初始化的Patches来控制生成视频的大小。

这个过程类似于在自然语言处理中，模型根据给定的token生成新的文本内容。通过这种方式，Sora模型能够根据需要生成不同大小和格式的视频，为视频生成和编辑提供了更大的灵活性和多样性。

### 5、视频生成 [扩展变压器](https://www.zhihu.com/search?q=%E6%89%A9%E5%B1%95%E5%8F%98%E5%8E%8B%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)

Sora模型的根基是建立在Transformer架构之上的扩散模型。

该模型通过接收输入的噪声 [Patches ](https://www.zhihu.com/search?q=Patches&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)和文本提示等调节信息，能够有效地预测出“干净”的Patch。

![](img/b8a241ed527c1be1add21eace760d4cb.png)

这种架构在大型语言模型、 [计算机视觉 ](https://www.zhihu.com/search?q=%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)和图像生成等领域都有着广泛的应用。

在训练过程中，使用固定的种子和输入，随着计算量的增加，生成样本的质量会显著提高。

这种训练方式使得Sora模型能够逐步学习并优化其生成能力，从而在处理视频和图像数据时，能够输出更加精细和逼真的结果。

## 四、原理讨论

### 1.个人理解

首先说下我个人的理解，Sora呈现的效果，只需要知道需求和结果，中间的原理并不是机器学习的内容。

也就是说，人工智能不需要知道中间的过程，中间的过程可能如同量子力学一样不可琢磨，它不需要知道具体的物理规律，而是如同孩童观察世界一样。

![](img/bba4c6f27340344ebaf3cf663aba100a.png)

孩童并不需要知道牛顿三大定律，而看到苹果从树上掉下来，就知道苹果必然会落地，而不是飞向天上。

下面是根据群友讨论的推测内容：

### 2.原理推测

而Sora让视频看起来符合物理规律主要依赖于其训练过程中使用的大量数据和先进的学习算法。

![](img/663838383fc6be4c22f759ac56fd235f.png)

通过在广泛的视频数据上训练，Sora学习到了世界如何运作，包括物体如何移动、互动和受到物理规律（比如重力和惯性）的影响。这个过程类似于人类学习物理规律：观察、学习和模仿。

### 3.数据和训练

Sora通过分析和理解大量包含物理互动的视频，学习到了物理规律的表现形式。

例如，它可以观察到苹果从树上落下来的视频，学习到重力的效应；

看到球在地面上滚动的视频，理解到惯性和摩擦力如何影响物体的运动。

通过这些观察，Sora能够生成新的视频，其中的物体和人物遵循现实世界的物理规律。

### 4.算法和模型架构

Sora使用的算法和模型架构（如扩散模型和变换器）使其能够在视频生成过程中考虑时间和空间的连续性。

![](img/16d9e59521b58d947231cf5805846e20.png)

这意味着它不仅能够理解单个画面中物体的位置和状态，还能够理解这些物体随时间如何变化和移动。

这种时空连续性的理解是让生成的视频看起来符合物理规律的关键。

## 结语

Sora通过分析大量的视频数据、学习物理规律的表现，并利用先进的算法理解和模拟时空连续性，从而能够生成看起来符合物理规律的视频。

这一过程涉及到复杂的计算和大量的数据处理，最终使得Sora生成的视频在视觉上既真实又符合逻辑。

AGI的未来或许真的不远了！

---

我是 [德里克文 ](https://www.zhihu.com/search?q=%E5%BE%B7%E9%87%8C%E5%85%8B%E6%96%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A682556788%7D)，一个对AI绘画，人工智能有强烈兴趣，从业多年的 设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！
