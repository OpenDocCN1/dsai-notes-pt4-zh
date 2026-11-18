
# 一图读懂Sora技术路径

> 来源：[https://datawhaler.feishu.cn/docx/WJgzdspiMoQsgJxx06CchEI2nig](https://datawhaler.feishu.cn/docx/WJgzdspiMoQsgJxx06CchEI2nig)

> 魔搭社区技术运营负责人成晨老师的一图读懂Sora技术路径：`https://mp.weixin.qq.com/s/xP46EocNg1x7IlTN_iF8kw`

## 引言

昨天，OpenAI发布了视频生成模型Sora，最大的Sora模型能够生成一分钟的高保真视频。同时OpenAI称，可扩展的视频生成模型，是构建物理世界通用模拟器的一条可能的路径。

Sora能够生成横屏1920*1080视频，竖屏1080*1920视频，以及之间的所有内容。这使得Sora可以兼容不同的视频播放设备，根据特定的纵横比来生成视频内容，这也会大大影响视频创作领域，包括电影制作，电视内容，自媒体等。

技术报告：`https://openai.com/research/video-generation-models-as-world-simulators`

## 技术解读

魔搭社区的开发者也针对Sora的技术报告展开了热烈的讨论，并根据技术报告的内容，推测了Sora的技术架构图如下：

![](img/e72cc164678ac87774d3ecf710a66ba4.png)

Sora的技术架构图（from魔搭社区开发者）

Sora模型的核心技术点（图中红色标注）：

### 视频压缩网络

OpenAI训练了一个降低视觉数据维度的网络。这个网络接受原始视频作为输入，并输出在时间和空间上都被压缩的潜在表示。Sora在这个压缩的潜在空间上进行训练，并随后生成视频。同时还训练了一个相应的解码器模型，将生成的潜在表示映射回像素空间（**源自Sora技术报告**）。这部分内容为图中的红色部分，核心工作为将视觉数据转化为patches，patches是从大语言模型中获得的灵感，大语言模型范式的成功部分得益于使用优雅统一各种文本模态（代码、数学和各种自然语言）的token。大语言模型拥有文本token，而Sora拥有视觉分块（patches）。OpenAI在之前的Clip等工作中，充分实践了分块是视觉数据模型的一种有效表示（参考论文：An image is worth 16x16 words: Transformers for image recognition at scale.）这一技术路线。而视频压缩网络的工作就是将高维度的视频数据转换为patches，首先将视频压缩到一个低纬的latent space，然后分解为spacetime patches。

![](img/763bce0d2f38773449946199b775f288.png)

图片来源：Sora技术报告

这个方法同样适用于图像（将图像作为单一帧视频处理），基于Patches的表示使得Sora能够训练具有不同分辨率，持续时间和纵横比的视频和图像，而在推理过程中，只需要在适当大小的grid中随机初始化patches即可控制视频生成的大小。

**技术难点：**视频压缩网络类比于latent diffusion model中的VAE，但是压缩率是多少，如何保证视频特征被更好地保留，还需要进一步的研究。

### 用于视频生成的Scaling Transformers

Sora是一个diffusion模型；给定输入的噪声块+文本prompt，它被训练来预测原始的“干净”分块。重要的是，Sora是一个Scaling Transformers。Transformers在大语言模型上展示了显著的扩展性，我们相信OpenAI将很多在大语言模型的技术积累用在了Sora上。

![](img/3bcb04078c55d27719db8dccbb74b87f.png)

图片来源：Sora技术报告

在Sora的工作中，OpenAI发现Diffusion Transformers作为视频生成模型具备很好的扩展性。

**技术难点：**能够scaling up的transformer如何训练出来，对第一步的patches进行有效训练，可能包括的难点有long context（长达1分钟的视频）的支持、期间error accumulation如何保证比较低，视频中实体的高质量和一致性，video condition，image condition，text condition的多模态支持等。

### 语言理解

OpenAI发现训练文本到视频生成系统需要大量带有相应文本标题的视频。这里，OpenAI将DALL·E 3中介绍的标题生成技术用到了视频领域，训练了一个具备高度描述性的视频标题生成（video captioning）模型，使用这个模型为所有的视频训练数据生成了高质量文本标题，再将视频和高质量标题作为视频文本对进行训练。通过这样的高质量的训练数据，保障了文本（prompt）和视频数据之间高度的align。而在生成阶段，Sora会基于OpenAI的GPT模型对于用户的prompt进行改写，生成高质量且具备很好描述性的高质量prompt，再送到视频生成模型完成生成工作。

例子（from Sora技术报告）

video：

，时长00:10

caption：an-old-man-wearing-a-green-dress-and-a-sun-hat-taking-a-pleasant-stroll-in-Mumbai-India-during-a-colorful-festival

，时长00:10

> caption：a-woman-wearing-purple-overalls-and-cowboy-boots-taking-a-pleasant-stroll-in-Johannesburg-South-Africa-during-a-beautiful-sunset

**技术难点：**如何训练一个高质量的视频caption模型，需要海量的高质量视频数据，包括数据的获取和标注，为了保障通用性，需要支持各种多样化的视频源，电影、纪录片、游戏、3D引擎渲染等等；标注工作包括对长视频的精准切片，以及切片后的captioning。中文高质量视频数据一直是稀缺资源，随着国内短视频业务发展，也许可以加快中文高质量短视频的收集和获取。

### 世界模型，涌现的模拟能力

当大规模训练时，sora同样也出现了有趣的“涌现的模拟能力”，这些能力使Sora能够模拟物理世界中的人、动物和环境的某些方面。这些属性没有任何明确的三维、物体等归纳特征信息——可以理解为由于模型参数足够大而产生的涌现现象。

这些能力包括：

+   **三维一致性** Sora可以生成具有动态摄像机移动的视频。随着摄像机的移动和旋转，人物和场景元素在三维空间中一致地移动。

+   **长距离连贯性和物体持久性** 对于视频生成系统来说，一个重大挑战一直是在采样长视频时保持时间上的连续性。研究发现，Sora通常能够有效地模拟短距离和长距离依赖关系（不稳定）。例如，Sora可以在人物、动物和物体被遮挡或离开画面时仍然保持它们的存在。同样，它可以在单个样本中生成同一角色的多个镜头，贯穿视频始终保持他们的外观。

+   **与世界互动** Sora可以模拟以简单方式影响世界状态的行为。例如，画家可以在画布上留下新的笔触，这些笔触随着时间的推移而持续存在，或者一个人可以吃汉堡并留下咬痕。

+   **模拟数字世界** Sora还能够模拟人工过程，一个例子是视频游戏。Sora可以通过基本策略控制《Minecraft》中的玩家，同时以高保真度渲染世界及其动态。这些能力可以通过prompt包含“Minecraft”，零样本激活这样的能力。

而这些能力都表明，顺着这个方向发展（持续扩大规模），Sora真的可能成为世界模型（能够高度模拟物理和数字世界的模拟器）。也许头部玩家，黑客帝国这些科幻片，就在不太遥远的未来。

**技术难点：**“大”模型，“高”算力，“海量”数据

## 总结

从Sora模型的技术报告中，我们可以看到Sora模型的实现，是建立在OpenAI一系列坚实的历史技术工作的沉淀基础上的：包括不限于视觉理解（Clip），Transformers模型和大模型的涌现（ChatGPT），Video Caption（DALL·E 3）等。正如社区的一位开发者说，虽然其中依然有非常多的技术细节OpenAI并没有披露，但是OpenAI画了一条“模糊”的路，有了这条模糊的路，大家就可以去尝试，从而画出通往视频生成的正确的清晰的路。

作为开源社区，我们期待视频生成领域的“LLaMa”，以及更加普惠的开源视频生成技术。
