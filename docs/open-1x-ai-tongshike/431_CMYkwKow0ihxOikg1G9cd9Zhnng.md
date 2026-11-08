# 从零入门AI生图原理&实践

> 来源：[https://datawhaler.feishu.cn/docx/SOrHdAfPxoWnxBxkIJ7cvIm2nOe](https://datawhaler.feishu.cn/docx/SOrHdAfPxoWnxBxkIJ7cvIm2nOe)

> **从零入门AI生图原理&实践 **是 Datawh**ale 2024** 年 AI 夏令**营****第**四期的学习活动（“AIGC”方向），基于魔搭社区“可图Kolors-LoRA风格故事挑战赛”开展的实践学习——

+   适合想 入门并实践 AIGC文生图、工作流搭建、LoRA微调 的学习者参与

>

> **学习内容提要**：从通过代码实现AI文生图逐渐进阶，教程偏重图像工作流、微调、图像优化等思路，最后会简单介绍AIGC应用方向、数字人技术（选学）

# 文生图基本常识

> 我们先简单了解一下文生图相关的基础知识，然后再开始相关的实践和进一步学习！

## 文生图的历史

文生图（Text-to-Image Generation）是一种通过文本生成图像的技术，其发展历程可以追溯到早期的计算机视觉和自然语言处理研究。这一技术的历史可以分为几个关键阶段：

| **发展阶段** | **发展介绍** |
| --- | --- |
| **早期探索** | 文生图的概念最早出现于计算机视觉和图像处理的早期研究中。 |
| **基于统计模型的方法** | 进入2000年代，随着统计模型和机器学习技术的发展，文生图技术开始得到更多关注。 |
| **深度学习的崛起** | 2010年代是文生图技术发展的一个重要转折点。随着深度学习，尤其是卷积神经网络（CNN）和生成对抗网络（GAN）的发展，文生图技术取得了突破性进展。2014年，Goodfellow等人提出的GAN模型通过生成器和判别器的对抗训练，极大地提升了图像生成的质量。随后，各类变种GAN模型被提出，如DCGAN、Pix2Pix等，使得文生图技术在生成逼真图像方面达到了前所未有的高度。 |
| **大规模预训练模型** | 进入2020年代，大规模预训练模型如OpenAI的CLIP、DALL-E以及Stable Diffusion等的出现，标志着文生图技术进入了一个新的时代。 |
| **更多关于文生图的****技术演变**，大家可以参考学习下面的视频 &amp; PPT。 |  |

## 文生图基础知识介绍

文生图主要以SD系列基础模型为主，以及在其基础上微调的lora模型和人物基础模型等。

![](img/8240f01aa0b82a1fe46a25adb34e834b.png)

接下来，我们简单了解下提示词、lora、ComfyUI和参考图控制这些知识点。

### 提示词

提示词很重要，一般写法：主体描述，细节描述，修饰词，艺术风格，艺术家

> 举个例子

> 【**promts**】Beautiful and cute girl, smiling, 16 years old, denim jacket, gradient background, soft colors, soft lighting, cinematic edge lighting, light and dark contrast, anime, super detail, 8k

> 【**负向prompts**】(lowres, low quality, worst quality:1.2), (text:1.2), deformed, black and white,disfigured, low contrast, cropped, missing fingers

### Lora

Stable Diffusion中的Lora（LoRA）模型是一种轻量级的微调方法，它代表了“Low-Rank Adaptation”，即低秩适应。Lora不是指单一的具体模型，而是指一类通过特定微调技术应用于基础模型的扩展应用。在Stable Diffusion这一文本到图像合成模型的框架下，Lora被用来对预训练好的大模型进行针对性优化，以实现对特定主题、风格或任务的精细化控制。

### ComfyUI

ComfyUI 是一个工作流工具，主要用于简化和优化 AI 模型的配置和训练过程。通过直观的界面和集成的功能，用户可以轻松地进行模型微调、数据预处理、图像生成等任务，从而提高工作效率和生成效果。

![](img/733eeec9f9e96a4fad6e64809e2203b5.png)

在ComfyUI平台的前端页面上，用户可以基于节点/流程图的界面设计并执行AIGC文生图或者文生视频的pipeline。

### 参考图控制

ControlNet是一种用于精确控制图像生成过程的技术组件。它是一个附加到预训练的扩散模型（如Stable Diffusion模型）上的可训练神经网络模块。扩散模型通常用于从随机噪声逐渐生成图像的过程，而ControlNet的作用在于引入额外的控制信号，使得用户能够更具体地指导图像生成的各个方面（如姿势关键点、分割图、深度图、颜色等）。

| **参考图控制类型** | **简介** | **示例** |
| --- | --- | --- |
| OpenPose姿势控制 | 输入是一张姿势图片（或者使用真人图片提取姿势）作为AI绘画的参考图，输入prompt后，之后AI就可以依据此生成一副相同姿势的图片； |  |
| Canny精准绘制 | 输入是一张线稿图作为AI绘画的参考图，输入prompt后，之后AI就可以根据此生成一幅根据线稿的精准绘制。 |  |
| Hed绘制 | Hed是一种可以获取渐变线条的线稿图控制方式，相比canny更加的灵活。 |  |
| 深度图Midas | 输入是一张深度图，输入prompt后，之后AI就可以根据此生成一幅根据深度图的绘制。 |  |
| 颜色color控制 | 通过参考图控制和颜色控制，实现更加精准和个性化的图像生成效果。 |  |

![](img/f8f8c7817de04cb79ecb2f3979b36c61.png)

![](img/2ae839fea4cec3742f7c9d7be295b098.png)

![](img/e6deb42f192ef513eed03714ac0b6aaf.png)

![](img/e72fb9f922b9318d643d136d5c2b15fc.png)

![](img/1173664acae27c8843b2dbff22d12b9e.png)

![](img/a0dc00dd8f9c8820294a2f08396751f0.png)

![](img/ef6a1bdb84b3f134dc159d1b90e9ecf9.png)

![](img/15e6df4e9eadbe8158b73f62d28ffb8c.png)

![](img/221092fa305b61746d91416edb442760.png)

![](img/ac7ebb18573e331c138440188db9852c.png)

> 现在，让我们来开展实践吧！开始通过代码完成模型微调&AI生图实践！

# 小白零基础 30 分钟 速通指南

> **教程贡献者说：**

> 我们整个学习活动，将带你从 **跑通最简的Baseline**，到**了解竞赛通用流程**、**深入各个竞赛环节**，**精读Baseline与进阶实践**，这些一个都不会少！

>

> 当前环节可让你在 **30****分钟**** ****内****，****从一**个完全小白的选手，跑通 “可图Kolors-LoRA风格故事挑战赛” ！

>

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在每次完成学习后，写笔记打卡哟~ 及时总结沉淀能让学习事半功倍~

## Step0：开通阿里云PAI-DSW试用

<https://datawhaler.feishu.cn/sync/DkaqdtRoIsuiyAbDBoPcacKUned>

## Step1：报名赛事！(点击即可跳转)

> 赛事链接：https://tianchi.aliyun.com/competition/entrance/532254

> 点击报名比赛，即可报名成功

![](img/39e79b1f31ff3e18011b152747c51660.png)

![](img/6d3947ae64bc6c51dd94af7d58a60c5e.png)

## Step2：在魔搭社区创建PAI实例！（点击即可跳转）

<https://datawhaler.feishu.cn/sync/AZmfdDSBGsaIxObZ2w4cStnKnid>

## Step3：30 分钟体验一站式 baseline！

1.  下载baseline文件**（大约需要2分钟）**

```
git lfs install
git clone https://www.modelscope.cn/datasets/maochase/kolors.git
```

![](img/8d7413b47d1353bde8e4e1d21c81bb36.png)

![](img/fd96f30ac13b21043255de88875a3250.png)

1.  进入文件夹，打开baseline文件

![](img/a93de9999331940e33f61dc29ee900c1.png)

![](img/dcbd6bb804a411bf816975e0606453a4.png)

1.  安装环境，然后重启kernel

> 安装 Data-Juicer 和 DiffSynth-Studio

> Data-Juicer：数据处理和转换工具，旨在简化数据的提取、转换和加载过程

> DiffSynth-Studio：高效微调训练大模型工具

![](img/635c7635d9efad648ec33f938b775988.png)

![](img/9387eea374cc3ca2dbff8225fb096d1b.png)

1.  **调整**prompt，设置你想要的图片风格，依次修改8张图片的描述（可选）

> 正向描述词：你想要生成的图片应该包含的内容

> 反向提示词：你不希望生成的图片的内容

![](img/298b6cfd79e6aa9178fea8c20667d027.png)

1.  依次顺序运行剩余的代码块，点**击代码框左上角执**行按钮，最终获得图片（大约需要20分钟）

> 下面的代码块按照功能主要分成这几类

1.  使用Data-Juicer处理数据，整理训练数据文件

1.  使用DiffSynth-Studio在基础模型上，使用前面整理好的数据文件进行训练微调

1.  加载训练微调后的模型

1.  使用微调后的模型，生成用户指定的prompt提示词的图片

![](img/831ecab3c0daf42c2e426e8ec0c55c07.png)

## Step4：微调结果上传魔搭（点击即可跳转）

> 链接：https://www.modelscope.cn/models/create

1.  移动结果文件

> 创建terminal，粘贴如下命令，回车执行

```
mkdir /mnt/workspace/kolors/output &amp; cd 
cp /mnt/workspace/kolors/models/lightning_logs/version_0/checkpoints/epoch\=0-step\=500.ckpt /mnt/workspace/kolors/output/
cp /mnt/workspace/kolors/1.jpg /mnt/workspace/kolors/output/
```

![](img/2e7fc69b9937f5da8cfcdeb57854fe22.png)

1.  下载结果文件

> 双击进入output文件夹，分别下载两个文件到本地

![](img/573c5905d006db4226b6c540e23dd4f4.png)

![](img/fd92016ce3561ad128e9c81cb823fafd.png)

1.  创建并上传模型所需内容

点击[魔搭链接](https://www.modelscope.cn/models/create?template=text-to-image-lora)，创建模型，中文名称建议格式：队伍名称-可图Kolors训练-xxxxxx

![](img/9bb4b0b4ca2505eb122eaa58da436640.png)

![](img/8e1ef9425094435c8d8da33ce955bada.png)

这样就算创建完成并发布在创空间讨论区了。

1.  来到[创空间](https://www.modelscope.cn/studios)，查看自己的模型是否发布

![](img/2092758bb475d7ffbf5dba6ec2753a60.png)

![](img/e7616993b7dd36651310391990ff1902.png)

1.  作品发布在[比赛品牌馆讨论区](https://modelscope.cn/brand/view/Kolors?branch=0&tree=11)（点击蓝色词块直接跳转）

+   发布标题格式为：天池平台报名队伍名称+LoRA模型链接地址+作品图（8图故事）

![](img/336c62e0b1c5f4273853fa7cdc5e367d.png)

![](img/07f11646e6be87f09411637d04ecd91c.png)

## 关闭PAI实例！！！！（点击即可跳转）

> 链接：https://www.modelscope.cn/my/mynotebook/authorization

> 运行完成后，别忘了回到魔搭，【关闭】实例，否则会一直消耗你的试用额度！

> 每小时消耗大概7个试用额度！（总共5000个，有效期三个月）

![](img/0b27f75b1905c93be9ec9ebfcdb9271a.png)

# 🎉 恭喜你生成了自己的第一个图片

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记写笔记、打卡&在群里接龙！

笔记标题带“Datawhale X 魔搭 A[I夏令营”](https://linklearner.com/activity/14/10/20)，发布到CSDN、哔哩哔哩、博客园、知乎、📕、👸号、掘金等公开平台，详情可参考学习者手册（电脑端点击即可跳转）

**优秀笔记案例参考：**

<https://datawhaler.feishu.cn/sync/VEPMd6cQoslOeObgNV6cO2hyngc>

![](img/60826f34060ed0bd34a5ca3907d4281e.png)

## 📅 然后迎接后续的挑战吧！

> **教程贡献者说：**

> 经过前面baseline的实操联系，相信大家已经成功获得了自己的第一个作品，那么我们现在一起来思考一下下面的这两个问题：

1.  这个图片的风格能不能修改，能不能微调出来一个其他风格类型的图片

> 大家好好思考这两个问题，下一节课我们会带着大家来学习这方面的内容

>

> **更多详情见如下学习规划表：**

| ## 从零入门 文生图原理&amp;实践 |  |  |  |
| --- | --- | --- | --- |
| **Task** | **Task 描述** | **相关知识点** | **学习安排** |
| **Task 1：****5****天** |  | 1.  赛事解读 | 开营直播 |
| **Task 2：****4****天** |  | 1.  使用通义辅助精读代码 | Task2直播分享 |
| **Task 3：****4****天** |  | 1.  赛事上分点1：comfyUI工作流 | 赛事解读与上分直播分享 |
| **选学****（2天）** |  | 1.  AI生图在市场中的应用介绍 |  |
|  |  |  |  |

> **打怪难度：简单**

1.  学习文生图相关原理

1.  熟悉[**魔搭**](https://modelscope.cn/)相关文生图工具&应用

1.  完成可图故事赛体验

1.  写笔记，完成 **Task1 **打卡

> **打怪难度：⭐️**

1.  精读**baseline和背景知识**、在群里分享思考

1.  学习如何使用AI辅助学习

1.  学习Prompt工程、微调

1.  写笔记，完成**Task2 **打卡

> **打怪难度：⭐️⭐️**

1.  精读 **baseline2**、在群里分享思考

1.  图像工作流、Lora微调基本原理

1.  写笔记，完成 **Task3 **打卡

> **打怪难度：⭐️⭐️**

1.  选学部分，没有打卡要求

1.  按需学习即可

### 祝大家开发顺利，学习愉快~ 多在群里提问和交流哟！

> 感谢[**魔搭社区**](https://modelscope.cn/)提供的学习参考内容以及相关工具：

+   AIGC专题视频课程：https://space.bilibili.com/1069874770/channel/collectiondetail?sid=3369551

+   AIGC专题品牌馆：https://www.modelscope.cn/topic/cf0de97eb6284e16812d7c54fbe29fe7/pub/summary

+   模型训练入口：https://modelscope.cn/aigc/modelTraining

+   赛事提交界面：[**h**](https://modelscope.cn/brand/view/Kolors?branch=0&tree=0)ttps://modelscope.cn/brand/view/Kolors?branch=0&tree=0

+   提问前建议先查看** **[ QA & 其他彩蛋-模板](https://datawhaler.feishu.cn/wiki/K6b1woVtlizjV0klBBgc6ZJUnie)** ****是否早已收录你的**问题~ （QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见：** **[从零入门AI生图原理&实践-学习者手册](https://datawhaler.feishu.cn/wiki/UM7awcAuQicI4ukd2qtccT51nug)

> 你还可以参考下面的学习方案，提升自己的学习效率——

+   定位自己的学习预期、设计自己的学习计划：每天花多少时间，做什么，大概能拿到什么结果

+   学习内容有很多，明确目标、按需学习自己所需要学习的内容即可

> （参考计划正在逐步更新中）

| **学习方案类型** | **知识&amp;能力基础** | **预期学习任务** | **学习内容 &amp; 所需时长** |
| --- | --- | --- | --- |
| **初级：60分，****提见识** | +   无 | 1.  知道怎么解读一个赛事 | 1.  [学习如何解读一个赛事](https://datawhaler.feishu.cn/wiki/CMYkwKow0ihxOikg1G9cd9Zhnng#part-KzfQdxy3OogDmUxvdbdc8OH8nPd) |
| **进阶：80分，****会应用** | +   熟悉 Python | 1.  通过代码出图 | 1.  [学习文生图原理](https://datawhaler.feishu.cn/wiki/CMYkwKow0ihxOikg1G9cd9Zhnng) |
| **高级：90+分，****能创新** | +   LoRA | 1.  通过代码制作高质量的图片 | 1.  跑通baseline |

| ### 个人学习计划（复制到自己的文档中自行填写即可~） |  |  |  |
| --- | --- | --- | --- |
| **序号** | **日期** | **今日目标** | **今日学习资料 &amp; 学习计划** |
| Day1 | 8月11日 |  |  |
| Day2 | 8月12日 |  |  |
| Day3 | 8月13日 |  |  |
| Day4 | 8月14日 |  |  |
| Day5 | 8月15日 |  |  |
| Day6 | 8月16日 |  |  |
| Day7 | 8月17日 |  |  |

# 进阶知识提要

## 零代码文生图Lora模型训练

> 刚刚我们体验了lora风格模型训练的全流程，现在给大家介绍一款界面化模型训练工具。无需任何代码，即可完成模型微调。

魔搭平台现推出的零代码图像模型训练工具，您只需提供几张图片，即可训练出专属的lora风格模型，并直接使用该模型生成新的图片。让我们一键开始使用魔搭的图像模型训练工具（点击跳转工具）

**训练步骤如下：**

![](img/20704ded87f88890c7990d2c6101ec3a.png)

![](img/131cc47a52742b43f9c32c57ef726cbb.png)

![](img/8d35596abf3124e579699b46c1c95d04.png)

> 注意📢：

1.  该模型训练工具上传的图片数量最多128张，相比于代码开发的模式，有一定的限制。故在模型效果上比我们上边代码生成的结果差。

1.  数据集例子使用了的“/mnt/workspace/kolors/data/lora_dataset/train/”文件夹中的部分图片，大家也可以自行选择一下其他的图片。

## 魔搭热门文生图AI应用集锦

现在我们来看看在文生图领域，有哪些好用的工具：

+   更多好玩有用的AI应用，大家可以去魔搭[AIGC专区](https://www.modelscope.cn/aigc/home)自己探索！

| **工具类型** | **应用名称** | **应用简介** | **体验效果** | **更多内容详见** |
| --- | --- | --- | --- | --- |
| **基础图片生成工具** | [**FLUX文生图模型体验空间**](https://www.modelscope.cn/studios/muse/flux_dev) | 该应用由**阿里muse团队**推出，是目前最强的开源文生图模型之一，在文字生成、复杂指令遵循和人手生成上具备优势。 | ![](img/6cac07cc33df4909ce646a064e719b54.png) | 消费级显卡，17G显存，玩转图像生成模型FLUX.1！ |
|  | [**可图文生图**](https://www.modelscope.cn/studios/AI-ModelScope/Kolors) | 可图是快手开源的一种名为Kolors（可图）的文本到图像生成模型，该模型具有对英语和汉语的深刻理解，并能够生成高质量、逼真的图像。 | ![](img/9748813f5c26fbb1ce29278ae469c77f.png) | AI绘画社区新成员：快手开源可图文生图模型，支持双语，可写中文！ |
|  | Stable Diffusion XL 1.0 | 所有文生图应用的祖师爷，不必多说，YYDS。继SDXL 0.9的有限、仅供研究发布之后，SDXL的完整版本是目前最好的开源图像生成模型。 | ![](img/e159297a6ccf638f7fcd3a7125a8f7f0.png) |  |
| **办公提效工具** | [**锦书 - 创新艺术字**](https://modelscope.cn/studios/WordArt/WordArt/summary) | 创意艺术字生成。通过自定义的概念，对文字进行变形和纹理生成，构建富有创意的个性化字形和纹理。 | ![](img/1733bb98ef5d8f7ac1c5d64257395f59.png) |  |
|  | [**创意海报生成**](https://modelscope.cn/studios/iic/PosterGenius/summary) | 做海报不求人！创意海报生成，自由设计，瞬间点亮创意生活。 |  |  |

![](img/6891597fdcbfc60d1e4b4e27b6c9baf4.png)

![](img/1d07ec07c023cc099ee73fc1d8c26fec.png)

## 来了解一下baseline代码的大致结构

> 简要概述代码做了什么

1.  **导入库**：首先，代码导入了需要用到的库，包括 data-juicer 和微调的工具 DiffSynth-Studio

1.  **数据集构建**：下载数据集kolors，处理数据集

1.  **模型微调**：模型微调训练，以及加载训练后的模型

1.  **图片生成**：调用训练好的模型生成图片

代码详情

1.  环境安装

```
!pip install simple-aesthetics-predictor

!pip install -v -e data-juicer

!pip uninstall pytorch-lightning -y
!pip install peft lightning pandas torchvision

!pip install -e DiffSynth-Studio
```

1.  下载数据集

```
#下载数据集
from modelscope.msdatasets import MsDataset

ds = MsDataset.load(
    &#x27;AI-ModelScope/lowres_anime&#x27;,
    subset_name=&#x27;default&#x27;,
    split=&#x27;train&#x27;,
    cache_dir=&quot;/mnt/workspace/kolors/data&quot;
)

import json, os
from data_juicer.utils.mm_utils import SpecialTokens
from tqdm import tqdm

os.makedirs(&quot;./data/lora_dataset/train&quot;, exist_ok=True)
os.makedirs(&quot;./data/data-juicer/input&quot;, exist_ok=True)
with open(&quot;./data/data-juicer/input/metadata.jsonl&quot;, &quot;w&quot;) as f:
    for data_id, data in enumerate(tqdm(ds)):
        image = data[&quot;image&quot;].convert(&quot;RGB&quot;)
        image.save(f&quot;/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg&quot;)
        metadata = {&quot;text&quot;: &quot;二次元&quot;, &quot;image&quot;: [f&quot;/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg&quot;]}
        f.write(json.dumps(metadata))
        f.write(&quot;\n&quot;)
```

1.  处理数据集，保存数据处理结果

```
data_juicer_config = &quot;&quot;&quot;
# global parameters
project_name: &#x27;data-process&#x27;
dataset_path: &#x27;./data/data-juicer/input/metadata.jsonl&#x27;  # path to your dataset directory or file
np: 4  # number of subprocess to process your dataset

text_keys: &#x27;text&#x27;
image_key: &#x27;image&#x27;
image_special_token: &#x27;&lt;__dj__image&gt;&#x27;

export_path: &#x27;./data/data-juicer/output/result.jsonl&#x27;

# process schedule
# a list of several process operators with their arguments
process:
    - image_shape_filter:
        min_width: 1024
        min_height: 1024
        any_or_all: any
    - image_aspect_ratio_filter:
        min_ratio: 0.5
        max_ratio: 2.0
        any_or_all: any
&quot;&quot;&quot;
with open(&quot;data/data-juicer/data_juicer_config.yaml&quot;, &quot;w&quot;) as file:
    file.write(data_juicer_config.strip())

!dj-process --config data/data-juicer/data_juicer_config.yaml


import pandas as pd
import os, json
from PIL import Image
from tqdm import tqdm


texts, file_names = [], []
os.makedirs(&quot;./data/lora_dataset_processed/train&quot;, exist_ok=True)
with open(&quot;./data/data-juicer/output/result.jsonl&quot;, &quot;r&quot;) as file:
    for data_id, data in enumerate(tqdm(file.readlines())):
        data = json.loads(data)
        text = data[&quot;text&quot;]
        texts.append(text)
        image = Image.open(data[&quot;image&quot;][0])
        image_path = f&quot;./data/lora_dataset_processed/train/{data_id}.jpg&quot;
        image.save(image_path)
        file_names.append(f&quot;{data_id}.jpg&quot;)
data_frame = pd.DataFrame()
data_frame[&quot;file_name&quot;] = file_names
data_frame[&quot;text&quot;] = texts
data_frame.to_csv(&quot;./data/lora_dataset_processed/train/metadata.csv&quot;, index=False, encoding=&quot;utf-8-sig&quot;)
data_frame
```

1.  lora微调

```
# 下载模型
from diffsynth import download_models
download_models([&quot;Kolors&quot;, &quot;SDXL-vae-fp16-fix&quot;])

#模型训练
import os

cmd = &quot;&quot;&quot;
python DiffSynth-Studio/examples/train/kolors/train_kolors_lora.py \
  --pretrained_unet_path models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors \
  --pretrained_text_encoder_path models/kolors/Kolors/text_encoder \
  --pretrained_fp16_vae_path models/sdxl-vae-fp16-fix/diffusion_pytorch_model.safetensors \
  --lora_rank 16 \
  --lora_alpha 4.0 \
  --dataset_path data/lora_dataset_processed \
  --output_path ./models \
  --max_epochs 1 \
  --center_crop \
  --use_gradient_checkpointing \
  --precision &quot;16-mixed&quot;
&quot;&quot;&quot;.strip()

os.system(cmd)
```

1.  加载微调好的模型

```
from diffsynth import ModelManager, SDXLImagePipeline
from peft import LoraConfig, inject_adapter_in_model
import torch


def load_lora(model, lora_rank, lora_alpha, lora_path):
    lora_config = LoraConfig(
        r=lora_rank,
        lora_alpha=lora_alpha,
        init_lora_weights=&quot;gaussian&quot;,
        target_modules=[&quot;to_q&quot;, &quot;to_k&quot;, &quot;to_v&quot;, &quot;to_out&quot;],
    )
    model = inject_adapter_in_model(lora_config, model)
    state_dict = torch.load(lora_path, map_location=&quot;cpu&quot;)
    model.load_state_dict(state_dict, strict=False)
    return model


# Load models
model_manager = ModelManager(torch_dtype=torch.float16, device=&quot;cuda&quot;,
                             file_path_list=[
                                 &quot;models/kolors/Kolors/text_encoder&quot;,
                                 &quot;models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors&quot;,
                                 &quot;models/kolors/Kolors/vae/diffusion_pytorch_model.safetensors&quot;
                             ])
pipe = SDXLImagePipeline.from_model_manager(model_manager)

# Load LoRA
pipe.unet = load_lora(
    pipe.unet,
    lora_rank=16, # This parameter should be consistent with that in your training script.
    lora_alpha=2.0, # lora_alpha can control the weight of LoRA.
    lora_path=&quot;models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt&quot;
)
```

1.  图片生成

```
torch.manual_seed(0)
image = pipe(
    prompt=&quot;二次元，一个紫色短发小女孩，在家中沙发上坐着，双手托着腮，很无聊，全身，粉色连衣裙&quot;,
    negative_prompt=&quot;丑陋、变形、嘈杂、模糊、低对比度&quot;,
    cfg_scale=4,
    num_inference_steps=50, height=1024, width=1024,
)
image.save(&quot;1.jpg&quot;)
```

## （赛事已结束）配套赛题解读表

| ## 可图Kolors-LoRA风格故事挑战赛 |  |  |
| --- | --- | --- |
| **赛事链接** | https://tianchi.aliyun.com/s/ce4dc8bf800db1e58d51263ff357d28f |  |
| **赛事任务** | 1.  参赛者需在可图Kolors 模型的基础上训练LoRA 模型，生成无限风格，如水墨画风格、水彩风格、赛博朋克风格、日漫风格...... |  |
| **评分规则介绍** | ### **1、主观评分** | ### **2、客观评分** |
|  | **此外，我们会核实选手上传的模**型文件，赛选手需提**交训**练的LoRA 模型文件**、LORA 模型的介**绍、以及使用该模型生成的至少8张图片和对应 prompt，以便我们能够复现生成效果，对于生成效果明显无法复现的，取消获奖资格。 |  |
| **任务提交格式说明** | 选手在天池平台后，需在魔搭平台上提交作品。步骤如下： |  |
|  | **1、**将训练好的LoRA 模型上传到[魔搭模型库](https://modelscope.cn/aigc/models?name=Kolors-LoRA-example-anime&amp;page=1) | **2、**作品发布在比赛品牌馆讨论区，[h](https://modelscope.cn/brand/view/Kolors?branch=0&amp;tree=11)ttps://modelscope.cn/brand/view/Kolors?branch=0&amp;tree=11 |
| **赛程安排** | **初赛：**报名后-2024年8月31日23:59 | **决****赛：**2024年9月5日答辩展示，线上决出一二三等奖（评委主观评分） |
| **奖项设置** | 奖项                     奖金 |  |
| **数据集分析** | 官方提供了示例数据集，但我们实际上可以自行寻找所需的数据集，用来训练自己的LoRA模型。 |  |

## 参考资料

1.  AIGC专题视频课程：https://space.bilibili.com/1069874770/channel/collectiondetail?sid=3369551

1.  AIGC专题品牌馆：https://www.modelscope.cn/topic/cf0de97eb6284e16812d7c54fbe29fe7/pub/summary

1.  模型训练入口：https://modelscope.cn/aigc/modelTraining

## 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/K6b1woVtlizjV0klBBgc6ZJUnie)！

> [ QA & 其他彩蛋-模板](https://datawhaler.feishu.cn/wiki/K6b1woVtlizjV0klBBgc6ZJUnie)任何想法都可以在学习群里交流或在   进行填写哟~

> 本教程主要由  、  写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。