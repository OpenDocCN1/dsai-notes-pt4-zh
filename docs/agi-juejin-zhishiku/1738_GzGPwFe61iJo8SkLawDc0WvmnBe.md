# Stable Diffusion 入门指南-3

> 来源：[https://agijuejin.feishu.cn/docx/GAkfdI7nGoOvwPxH9B1clThUnOg](https://agijuejin.feishu.cn/docx/GAkfdI7nGoOvwPxH9B1clThUnOg)

> 作者：德里克文

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/646294193)hihu.com/p/646294...

这一篇文章我来讲解下在大模型的分类，关于前两篇介绍请点击这个链接查看：

[S](https://zhuanlan.zhihu.com/p/645820130)table Diffusion 入门指南-18 赞同 · 1 评论文章 31 赞同 · 1 评论 文章

[  ](https://zhuanlan.zhihu.com/p/646079558)

**大模型类别**

在入门指南-1中，我已经初步介绍了模型的分类和他们的类型，接下来我来推荐下我常用的模型分类及下载方法，小伙伴们掌握后，可以自己去挑选感兴趣的模型下载。

## **一. 模型下载**

### 1.下载地址

Stable Diffusion最著名和通用的模型下载地址是：

https:// civitai.com/  （需要魔法），里面包括各类模型下载,俗称C站。

国内的模型分享平台包括：

[  ](https://link.zhihu.com/?target=https%3A//tusi.art/)

[  ](https://link.zhihu.com/?target=https%3A//www.liblibai.com/)

### 2.存放目录

点击进入网站，下载需要的模型文件后，需要放置不同的模型到各自的目录。

![](img/b9842d88ab80da4247dcd6750c0b5255.png)

Checkpoint模型存放位置： SD主程序目录位置/models/Stable-diffusion

**VAE模型存放位置： **SD主程序目录位置/models/VAE

**Embedding存放位置： **SD主程序目录位置/embeddings

Hypernetworks存放位置： SD主程序目录位置/models/hypernetworks

**Lora存放位置： **SD主程序目录位置/models/Lora

## 二.Checkpoint大模型介绍

Checkpoint是主模型，是决定整体出图质量最重要的模型。每个Checkpoint大模型都有自己独特的风格，按照我自己的喜好分类，我将模型分成这四大类：真实照片类、2.5D仿真画风类、CG动画画风类、二次元平面画风类（另外还有一个NSFW类，因为经常容易渲染出各种限制级画面，我单独分类，这就不展开说了）。

![](img/87e26aadbb5faae3998812e7ec4a1a11.png)

Checkpoint文件是训练过程中保存的模型状态快照，我们可以理解为一种风格滤镜，用于将模型输出结果转换为特定的风格。在下载Checkpoint文件时，建议查看下相应的模型简介和说明事项，作者通常会提供文件和对应的使用说明，以帮助使用者更好地理解和使用。

我推荐我常用常用的Checkpoint模型及不同的分类（对于初学者来说，建议每一种选我给的前3个即可)：

### **真实照片类**

Chilloutmix(NSFW),

ReallisticVision,

**Deliberate,**

**（MajicmixRealistic, br**aBeautifulRealistic , Lofi, ProtogenX34Photorealistic,）

![](img/522ae02abcdd666269ae0a85db88bad0.png)

### **2.5D仿真画风类**

**3Guofeng3,**

**ghostmix,**

**Dreamshaper,**

**(lyriel, Neverendi**ngDream,ProgenAnime）

![](img/8aa3de9356cd51b06a682d1711e50165.png)

### **CG动画画风类**

**Abyssorangemix;**

**CetusMix,**

DisneyPixarCartoon,

**DarksushiMixMix**

![](img/0f71091859ffc9ca3d04973a33a93546.png)

**二次元平面画风类：**

**Anything,**

**Counterfeit,**

**MixProColorbox,**

**(Meinamix,CardosAnime**,DreamlikeDiffusion)

![](img/9158fa3fcadd1b9fad480089416b0e74.png)

## 三. VAE模型介绍：

VAE 可被看作对模型的微调滤镜，主要给人的直观感受是色彩方面的调整，不同的VAE可能在细节上有一些差异，但不会对输出效果产生影响。

![](img/143393b5891dd85d72fe349ac170a961.png)

VAE它可以提高模型的性能，有些模型预设了VAE效果，因此除了模型配套的VAE文件外，无需专门下载，可以选择自动模式进行匹配，即可正常使用。

![](img/c133e10e409876c8fb2825032ad683dc.png)

## 四. Embedding介绍：

在Stable Diffusion中，Embedding就像是一本使用说明，它能够将输入数据转换为向量表示，使得模型能够更方便地进行处理和生成。

如果拿菜谱举例，类似于我们想要做一份地三鲜，在没有Embedding的情况下，我们在提示词里需要提到所有的细项特征，比如茄子、土豆、青椒、耗油、盐、香油、蒜、姜等配料。但是如果我们有了Embedding，我们只需要在提示词中输入：“地三鲜”，系统就能够通过Embedding自动读取关于地三鲜的相关所有详细特征描述，这样我们就不需要每次都输入很多单词来控制生成的画面了。

![](img/f2a306c476ffd4b409efa46ec9f52ed9.png)

调用Embedding需要在提示词中进行描绘，这样就能启用相应的Embedding效果了。

Embedding与其他模型（比如LORA）相比，Embedding文件的大小只有几十KB，而不是几百兆或几GB。虽然还原度可能相对LORA稍差一些，但在存储和使用方面更加方便。

需要注意的是有些Embedding是用在正面提示词，有些是用在负面提示词，例如 EasyNegative，verybadimage这几个是用于负面的

![](img/84f52fcb16eddb466b70b39c01e40bac.png)

**我这里也介绍一些实用的Embedding: EasyNeg**ative，ng_deepnegative,verybadimagenegative

## 五.Hypernetworks（略过)

这个模型现在基本没人用，又麻烦效果上也比不上Lora，我就不介绍了，对于日常应用，忽略就好。

## 六.Lora介绍

Lora与embedding在本质上有相似之处，都携带着大量的训练数据。因此，LORA能够更加细腻地复制人物和细节特征。

### 1、启动Lora

通常每个Lora模型都有对应训练的Checkpoint大模型和相应的触发词汇，我们可以通过查看LORA作者提供的相关内容来获取信息，并选择一些提示词，来更好的生成图片。

![](img/d18c854572b42be7a092ab9ad82392bb.png)

调用Lora需要在提示词中进行描绘，Lora也可以叠加重复采用，例如我在下面的图片示例的，在提示词中加入 <lora:画风LoRA:GhibliStyle:0.8>, <lora:add_detail:1>这样就能启用相应的Lora效果了，0.8和1则分别代表了这个的Lora启用的权重。

![](img/0fb9d7e87064493a7e1daba0cc581e3a.png)

需要特别注意，每个LORA模型都可以在提示词中设置权重属性。权重设置越大，对画面的影响就越大。一般来说，我建议Lora提示词的权重保持在0.3到1之间。权重过低没有效果，权重过高，有可能出现过于拟和，影响出图质量。

我这里也介绍一些我自己常用的的Lora: add_detail,blindbox,animeiLineart

这几个Lora分别有什么效果呢，让我简单介绍下吧

### **2. add_detail**

有了这个Lora，可以给你的画面增加更多的细节,我来用图片举个例子吧。

下面两张图片我用了相同的提示词和参数，仔细看看不同在哪里，细节更加丰富，错误更少了。上方的图是没有加add_detail这个Lora的，下方的图我设置add_detail权重为1，是不是就精致多了呢？你也可以自己试下。

![](img/1477ebb4eb6cb5424a2911e6345cd5f1.png)

### **3.blindbox**

最好用的生成盲盒类LORA，可以直接当生产力使用，不需言语，就是强烈推荐

![](img/2e1dafd1eb7076fa3ac2bb521fa17ae6.png)

### 4.animeiLineart

这个Lora最大的好处就是可以生成线稿图啦，可以很轻松制作漫画风格哦

![](img/952a349633cbdf552c5df208e99d40b1.png)

好了，关于模型分类你是否了解了呢，如果不清楚可以评论区留言给我哦，我会进行解答。

下一篇文章我们将学习关于如何更好优化提示词及重要插件Controlnet的使用，想要了解学习AI绘画，请继续关注我的作品吧。

---

我是德里克文，一个对AI绘画，人工智能有强烈兴趣，从业多年的室内设计师！如果对我的文章内容感兴趣，请帮忙关注点赞收藏，谢谢！