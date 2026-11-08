# 2023年主流Ai绘画工具使用指南

> 来源：[https://aiyanxishe.feishu.cn/docx/W8CVdNLSuoaD3Cxlbxfc7ZUMnZc](https://aiyanxishe.feishu.cn/docx/W8CVdNLSuoaD3Cxlbxfc7ZUMnZc)

## **Ai****绘画是什么？**

![](img/4bc7f092cff466e95570b20a933b12d2.png)

Ai是一种计算机生成绘画的方法，它使用人工智能算法来创作绘画。

为了更快了解到Ai绘画，先抛去理论，算法，直接从实现“Ai绘画”的工具中了解。

目前的Ai绘画工具有：Disco Diffusion、Stable Diffusion、Novel Ai、Tiamat

为了能知道多一点，我划分为“操作门槛、生成质量、风格定性、版权声明”来体验。

### Disco Diffusion

![](img/e35b237cdcf554396cb06161c039841b.png)

开发商：谷歌

体验网址：[ht](https://link.zhihu.com/?target=https%3A//colab.research.google.com/drive/1Ce-990CRb7AXvaSRV0DvhVFF8E9ogCaI%23scrollTo%3DCreateVidTop)tps://colab.research.google.com/drive/1Ce-990CRb7AXvaSRV0DvhVFF8E9ogCaI#scrollTo=CreateVidTop

教程：[https://www.zcool.com.cn/articl](https://link.zhihu.com/?target=https%3A//www.zcool.com.cn/article/ZMTM3OTg3Mg%3D%3D.html)e/ZMTM3OTg3Mg==.html

操作门槛：适中 需要🪜与谷歌账号，操作界面略难上手，需要按照教程一步一步生成画作。

![](img/d6dce09a63b098ea90d9ba38cbf5ee62.png)

Ai主要根据描述，描述需要英文（识别关键词进行千种图形拼接）。

生成质量：细节满满，颜色丰富鲜艳。非常富有意境~

以下图片就是使用了6~15分钟生成的Ai画作。

![](img/53f395e3bbaab58e730b4769ac0825e9.png)

我翻译了关键词的内容：

![](img/dba7d3d6b1d5bf89ea644dbcbdc218ae.png)

关键词的目的是为了方便让Ai能凭借关键词从海量的数据中抓取相关的图片进行拆分，

提取再重组。

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将yellow color scheme修改为green color scheme。

结果生成如下：

![](img/1915c29ab9a97017d3227793ed037662.png)

再将“a singular lighthouse（灯塔）”修改为“island（岛）”

结果变成为：

![](img/28d80648f935c22c8f46de3b645aadac.png)

最后再将“greg rutkowski and thomas kinkade（两位画家的名字）”修改为“Vincent Willem van Gogh（梵高）”

结果变成为：

![](img/7a25a5c2ecc698235100bfb24fa527d8.png)

从这三个尝试中，虽然还不知道有那些关键词能控制Ai绘画的最终效果，但初步推测Ai判断关键词的时候，会将关键词进行优先排序，越易受大众所理解的关键词，Ai会排得越前，而排在越前的关键词越能影响画作整体效果。

风格定性：Disco Diffusion生成的画作，有着浓厚的油画风。明显的色块与笔刷痕迹，非常贴合超现实概念画。

版权声明：Disco Diffusion是完全免费开源的工具，遵循MIT开源协议，在目前声明内容中，工具的使用是开放的，但在生成作品是否可商用或是版权归属仍是模糊地带，它更像是PS、Ai一样，只提供制作工具，而作品版权问题或责任交给创作者。

![](img/22182e1f72d126a7ae9b754ed2587732.png)

### Stable diffusion

![](img/f1260f6112b889904039680e2bbdc1ec.png)

开发商：DreamStudioAI

体验网址：https://beta.dreamstudio.ai/dream

教程：[h](https://link.zhihu.com/?target=https%3A//baijiahao.baidu.com/s%3Fid%3D1743758097302942643%26wfr%3Dspider%26for%3Dpc)ttps://baijiahao.baidu.com/s?id=1743758097302942643&wfr=spider&for=pc

操作门槛：简单 UI界面直观

![](img/63a047fc398ecf4f9d8dd58b434fba33.png)

![](img/9c093a6833810227ebd240cc04ba0277.png)

画作生成的关键依旧是根据描述词。这里还是给DreamStudio点个赞的，尤其是从Disco Diffusion切换过来，感觉去掉了许多繁琐的代码程序，浓缩成几个直观的UI控件。

而且还多了“配置比例、脚本、采样器、模型、种子、图片”~

生成质量：生成的画作更贴合输入的描述。

为了更好的识别Disco Diffusion与stable diffusion的差异。我依旧会使用相同的描述生成出来的画作进行对比。

![](img/aef576375341dc1d96283f0f92c895ce.png)

以下图片就是使用了2~5分钟生成的Ai画作。

![](img/e7d359dc539e7af285a4f2d871c9eca3.png)

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将yellow color scheme修改为green color scheme。

结果生成如下：

![](img/5ac947f2b382eac4030d0f7301720fb8.png)

再将“a singular lighthouse（灯塔）”修改为“island（岛）”

结果变成为：

![](img/952370444c0121ffa879fd89b426b36d.png)

最后再将“greg rutkowski and thomas kinkade（两位画家的名字）”修改为“Vincent Willem van Gogh（梵高）”

结果变成为：

![](img/6a527c27a752b6215982434792307632.png)

从这三个尝试中，我再一次感叹Ai绘画的超神，与Disco Diffusion超现实概念画不同的是，stable diffusion的拼接感几乎没有感觉出来，确实达到真实作画的效果。而且它最恐怖在于，它对关键词的理解更为精准。搭配它快速生成的时间，可以是证实Ai的威力。（当然，我还是蛮喜欢Disco Diffusion生成，它给人的联想空间会更大一些，感官刺激也会大一些。）

风格定性：stable diffusion生成的画作风格是依据关键词，没有特定风格绑定。

版权声明：与Disco Diffusion不同的是，stable diffusion现阶段不是开源工具，所以在版权声明中，它不会指工具的使用权，而是生成图形的版权。从以下信息中，能得知stable diffusion

生成的画作版权不会归它所用。归创作者所有，but！目前AI作品的版权条例仍是未知，

这不意味着画作就无版权纠纷，它的版权纠结是与现有版权的画作（影像作品）纠纷。而非是对于Ai绘画的保护。

![](img/285d373cc857eab9dc0c55a19e0e81e7.png)

![](img/c15274cb284ea9c2724546f27acb12ac.png)

### Novel Ai

![](img/2f39fc0dfbb9ee919d0d7a4e666944f3.png)

开发商：NovelAI

体验网址：https://novelai.net/

教程：[https://www.yuque.com/](https://link.zhihu.com/?target=https%3A//www.yuque.com/zhengchuanmao/nn7hgw)zhengchuanmao/nn7hgw

操作门槛：简单 UI界面简单

![](img/f17b9b91b4db70f94a59b8a86d65b6d1.png)

我发现只要是有UI界面的AI绘画工具，功能模块都是大相径庭。

而Novel Ai与其他两款Ai绘画工具不同的是，Novel Ai生成图像是没有试用次数的，需要充值点数才可生成。

（最低10美元/1000点起充，由于需要PayPal进行充值，就淘宝来充了）

基本在85~95元不等。

（这里还有一个小贴士，再作操作的时候，需要关掉浏览器翻译，不然页面会丢失）

生成质量：为了更好的识别NovelAI与Disco Diffusion与stable diffusion的差异。我依旧会使用相同的描述生成出来的画作进行对比。

![](img/2dc2312a1f106ac1ee1fd718e5c2b2f6.png)

以下图片就是使用了2~5分钟生成的Ai画作。

![](img/3882524749eaee75359ca76c15a89982.png)

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将yellow color scheme修改为green color scheme。

结果生成如下：

![](img/3f36d3b37deeb500b3eb3aa4d085d562.png)

再将“a singular lighthouse（灯塔）”修改为“island（岛）”

结果变成为：

![](img/06b7b3dffa0edeabdc3942245798ed00.png)

最后再将“greg rutkowski and thomas kinkade（两位画家的名字）”修改为“Vincent Willem van Gogh（梵高）”

结果变成为：

![](img/c9d71c60a50cbd168f6ebdac0f905607.png)

从这三个尝试中，NovelAI生成出来的画作更为具体，可以说是能达到原画级别。对于关键词判断来说，颜色的英文单词更容易视为烘托氛围的指令。

与Disco Diffusion相比，多了一种更具象的视觉呈现。

与stable diffusion相比，少了一种不同风格之间的变化。

风格定性：偏向写实油画，有一种唯美感。

版权声明：与Disco Diffusion一样主张工具用途，而非定义为Ai图形平台，正如NovelAi不会将生成图像存储在服务器上这一条，它们也知道Ai画作的版权模糊。

![](img/3acbbaa5748d87cf1e9b24ff7498ce93.png)

### Tiamat

![](img/6c29e5be2b35d51367afb8a1ab145f0e.png)

开发商：上海退格数字科技有限公司

体验网址：网页端目前只限内测名额使用，不过可以搜索“TIAMAT AI”🛰️小程序体验一下。

教程：[https://www.bilibili.com/video/BV1rm4y1F7V7/?spm_id_fr](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1rm4y1F7V7/%3Fspm_id_from%3Dtrigger_reload%26vd_source%3D2de83af4a66e0414b8fc2f1c598a80a6)om=trigger_reload&vd_source=2de83af4a66e0414b8fc2f1c598a80a6

操作门槛：哈哈哈，感觉但凡是国人涉及到的领域，都会使之步伐加快很多。

从界面、操作来看，TIAMAT AI小程序都十分的友好，易上手。

![](img/01f7226b943ee9c7fe8f7fd2a59ee183.png)

![](img/a24167e67717f0cc04d03873a3bd97e2.png)

而且在引擎上还进行了划分，覆盖不同类型的创作需求。

这一点，是前三者所没有的，最关键的是TIAMAT AI是支持中文关键词的录入，

从用途、操作，这款AI更接近商业的应用。

生成质量：TIAMAT AI貌似是不支持英文关键词。

![](img/e9f11076861dba883fcb720ce3eb3a22.png)

但为了有对比性，我将之前测试的关键词翻译成中文进行测试。

![](img/8ae7f2f5da33c5c7a7f4072becf8bf5f.png)

以下图片就是使用了2分钟生成的Ai画作。

![](img/f9661cfb570496a23965ccbbbd4986ee.png)

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将黄色配色方案修改为绿色配色方案。

结果生成如下：

![](img/e160e8c48b4963fc5703ef617d4dd387.png)

再将“灯塔”修改为“岛”

结果变成为：

![](img/2a5892c570421f6ff00bf3275f302adc.png)

最后再将“格雷格·鲁特科夫斯基和托马斯·金卡德”修改为“梵高”

结果变成为：

![](img/14b182a2e69cca55f2aeec18840105aa.png)

天~~哈哈哈，tiamat生成的效果叹为观止，从关键词的修改中，tiamat是在三个Ai绘画工具中，更贴合关键词描述的。更像是前三者的进化版，非常的精细，而且没有固定风格硬性捆绑。

风格定性：写实，且更接近于完成品的精细程度。

版权声明：从协议中大致能看出Tiamat把用户定义为“体验者”，所有在Tiamat创作的内容归Tiamat所有，如需进行使用或传播需要经过官方同意，从这点可以规避如用户创作出现侵权行为，首要的撇清了平台的责任。（你虽然做了，但没有得到我的允许，你的责任需要自行承担），更细的一步就是未经平台允许在网上传播的作品本身就已经是违规的。

![](img/13f265d0b7f276818bc305396e431241.png)

### Midjourney

![](img/1cc9eb2cafbf97c8949747182b6f4caf.png)

开发商：midjourney

体验网址：[ht](https://link.zhihu.com/?target=https%3A//www.midjourney.com/home/%3FcallbackUrl%3Dhttps%253A%252F%252Fwww.midjourney.com%252Faccount%252F)tps://www.midjourney.com/home/?callbackUrl=https%3A%2F%2Fwww.midjourney.com%2Faccount%2F

教程：[htt](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV16d4y1A7Zq/%3Fspm_id_from%3D333.337.search-card.all.click%26vd_source%3D2de83af4a66e0414b8fc2f1c598a80a6)ps://www.bilibili.com/video/BV16d4y1A7Zq/?spm_id_from=333.337.search-card.all.click&vd_source=2de83af4a66e0414b8fc2f1c598a80a6

操作门槛：简单 只需要输入指令+关键词到指定的聊天室即可

![](img/def50f42d6023ab2a82028f885101b76.png)

不同于以上的Ai绘画平台，midjourney是植入在Discord里，可以说更像是一个聊天室，没有繁琐的参数设置，

更直接的输入关键词生成图像。

为了更好的识别midjourney与上面Ai绘画平台的差异。我依旧会使用相同的描述生成出来的画作进行对比。

![](img/edf2a7cfd0f82265520ae32eaee0bafd.png)

貌似关键词有违规词导致生成失败，我再删除违规词，试一下。

以下图片是一分钟内生成的Ai画作

![](img/ac43d228fc5aa82891fa9f4bc5332c9e.png)

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将yellow color scheme修改为green color scheme。

结果生成如下：

![](img/bdf0a61c714ac72d88255888545ef359.png)

再将“a singular lighthouse（灯塔）”修改为“island（岛）”

结果变成为：

![](img/c6d85fa9ecc5632ae4dc692fb9439d09.png)

最后再将“greg rutkowski and thomas kinkade（两位画家的名字）”修改为“Vincent Willem van Gogh（梵高）”

结果变成为：

![](img/588da858dffd34366d8ff578a2424210.png)

风格定性：游戏插画

生成质量：到了midjourney这里，与其说它对你的关键词按顺序的拆解绘制，还不如说它自动的把你可能会喜欢、日常接触的风格给呈现出来，它少了许多粗糙感，多个许多精致的地方。与上面相比，它生成的Ai绘画难以辨别为是Ai画的。

版权声明：midjourney植入在Discord，以聊天室的形式存在，其实蛮聪明的作法。既然Ai画作的版权目前无法有具体细节的明文规定，与其把麻烦自己揽在身上，还不如把这个麻烦作为公众监督的视窗，所以说拿Ai改成自己的画不会有人知道这个说法严格意义上是不存在的。只要你是在midjourney平台生成，就会有人能看过你生成的图，从而变成证据。

另外Ai画作它在midjourney里没有“独家”的说法，任何人都可以拿你生成的作品进行二次改造。这也是Ai画作的缺点吧。

![](img/db354e4bfa038d8817afba136d52c47b.png)

### nijijourney

![](img/9e02573f49923446cc9a2d6aecec159c.png)

开发商：Spellbrush与Midjourney

体验网址：https://nijijourney.com/zh/

教程：[htt](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1sT411Q7X1/%3Fspm_id_from%3D333.337.search-card.all.click%26vd_source%3D2de83af4a66e0414b8fc2f1c598a80a6)ps://www.bilibili.com/video/BV1sT411Q7X1/?spm_id_from=333.337.search-card.all.click&vd_source=2de83af4a66e0414b8fc2f1c598a80a6

操作门槛：简单 只需要输入指令+关键词到指定的聊天室即可 （它就是midjourney的姐妹版）

![](img/f50363fa247a8e55de802693ae97909a.png)

![](img/8d1d7ccf6492253fb7b7799e3c17b680.png)

既然是姐妹版，它和midjourney一样是植入在Discord里，直接输入指令+关键词生成Ai画作。

为了更好的识别nijijourney与midjourney的差异。我依旧会使用相同的描述生成出来的画作进行对比。

以下图片是三分钟内生成的Ai画作

![](img/83725978d1da0eb2bab4efc590807102.png)

为了浅探AI绘画如何根据关键词（主次顺序？）进行生成画作，我从关键词修改着手

首先将yellow color scheme修改为green color scheme。

结果生成如下：

![](img/851d9c3e07b11e5c3d6f94b20f0661b4.png)

再将“a singular lighthouse（灯塔）”修改为“island（岛）”

结果变成为：

![](img/ad1de4900a166794c43efe92da67f41a.png)

最后再将“greg rutkowski and thomas kinkade（两位画家的名字）”修改为“Vincent Willem van Gogh（梵高）”

结果变成为：

![](img/9a0137e6e858bb253dd1f7a5cf936abb.png)

风格定性：略带有二次元风格的感觉

生成质量：从整体看起来，其实它与midjourney的生成的Ai画作差异不会很明显，只能说nijijourney生成出来的没midjourney那么有游戏感。还有就是最后生成的结果有一张“有点二次元”的味~

本文由[**AI研习社**](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf)整理发布，AI研习社是一个AI行业研究专题知识兴趣小组，目前分享包含： AI工具使用、AI商业模式研究、AI行业研究报告、AI经验分享、大厂动态等专题内容，包含ChatGPT、MidJourney等主流工具使用说明，通过知识手册的方式帮助大家构建系统化的AI知识库，让更多人可以轻松学习使用AI技术。

![](img/a27ba9c9282afb28c2c91f009d528dd0.png)

扫描二维码，了解[**AI研习社**](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf)** **