# 微学习18 | Deepseek+coze：生成新闻播报

> 来源：[https://datawhaler.feishu.cn/docx/ZfI4d7i0oo5b1kxFOvncZevWn6c](https://datawhaler.feishu.cn/docx/ZfI4d7i0oo5b1kxFOvncZevWn6c)

# **效果展示**

![](img/88c72d8acff96307275da9fc36d2caa1.png)

[1ee81ed2-5732-4fee-a5b3-c774c6372b18.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ic0Cb09VVoiOpLxCoY3cmOQrnhh)

![](img/1d477e63a2a8fa3168370a4fd567ea75.png)

[b72bec51-f6e7-4cbc-adb9-50a51f183e95.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DbpdbVNymo2uZ4x4t6dcdS3YnPe)

**操作演示在最后！！！**

# **✨ 灵感来源**

**近期观察到 Coze 平**台上出现了多种视频自动化视频玩法，如认知觉醒、英语单词学习等。本文将介绍如何通过 DeepSeek + Coze 实现新闻播报的自动化生成工作流，让我们能够仅通过输入新闻链接，即可一键生成图文并茂的短视频新闻。

**那么这样视频是怎么做到的？不需要手**动去采集新闻和剪辑，而是用扣子，花不到30秒的时间和0.0几元的成本（也有不花钱的方案，后面介绍）自动完成

![](img/d098b22f49ecc46e35db989c49614b2b.png)

**今**天带着大家Step by Step 创建一条 deepseekR1 + coze 实现的自动新闻视频实用工作流吧！

希望大家在2025，能用AI简化自己的工作，降本增效！⭐

**整个工作流分为四大模块：**

1.  内容获取与处理

1.  画面生成

1.  语音合成

1.  视频合成

![](img/091a72e6513e3080c08aa4cc81cf4d67.png)

# **🎯 核心功能**

这套系统能够实现：

1.  一键提取新闻内容

1.  智能生成播报文案

1.  AI 语音合成播报

1.  自动化视频制作

1.  全流程无人工干预

# **🌟 工作流程详解**

### **第一步：内容获取**

![](img/86f2749048f0d21d9da5d8ac87ea6c21.png)

只需输入新闻链接，系统就能自动提取核心内容

开始节点，入参：1、新闻链接 2、视频合成插件api_key

![](img/4537e17716cf61eb8d8f1d34aa12799d.png)

添加网页图片链接提取插件，承接开始节点的新闻链接

![](img/5e50bc46d8c00aeeedcf9540cc2e70da.png)

获取网页里的图片，我们这里以1ai.net的资讯为例子，这里的资讯都比较简单，只有一张主图，标题，内容都齐全，所以适合我们来操作

![](img/5bfc9afed7d4e8fb5ad9fe5c98188712.png)

**当我们输**入1ai.net的一则新闻，发现，提取出很多链接

![](img/97b5c4c4e74fa598b2ae96ae8e6d7639.png)

有趣的是，**第一条链接就是新闻主图**，其他内容都是不重要的icon

![](img/037b6a397c5edc8cdbc9de6f29c1fde5.png)

![](img/adb3d30a8b0db9757c8dc9638725bbbf.png)

所有我们就要**添加图片链接提取节点**，如果我们为了**节省写代码时间**，直接用大模型节点帮我们提取，只拿提取的链接集合的第一条，这样新闻的主要图片就搞定了。

![](img/7a9058347304943da5f51406770cf120.png)

![](img/1c34275c21ef2ea97cb541b397016684.png)

图片链接提取提示词

提取json格式{{input}}所有链接中的第一个链接，直接给出干干净净的url链接，比如：https://www.1ai.net/wp-content/uploads/thumb/2025/02/fill_w720_h480_g0_mark_9371bfaaj00sryokh002cd000p400eop.jpg

```
```

接着利用，**调**整图片的节点，将url属性的图片内容转化为image属性的图片（因为url节点在画板中是string的格式，所以必须转为img格式）

![](img/80e448819c5825af73b5c57c9b5c4eb2.png)

现在到了文字部分，**使用链接读取节点**，将文字内容提取出来

![](img/ca46cf311ff65b25ccadb29f5101e559.png)

在提取链接后面接上一个大模型节点，用来重写新闻成为口播稿子，这里用到最强的DeepseekR1模型，来生成有吸引力的口播内容

![](img/ad87be49fb4e50e44736f899c63d47cf.png)

**小**tips，如果想要加上自己的特征，可以在提示词里写：”开头加上‘这里是伊登AI’之类的个性化台词防伪“

```
# 角色
你是一位经验丰富、创意十足的新闻主编，同时也是备受关注的爆款新闻短视频博主，对新闻热点有着极其敏锐的洞察力。

## 技能
### 技能 1: 生成新闻视频口播内容
1. 仔细且全面深入地剖析{{input}}文章内容，精准提炼出核心要点。
2. 以极具吸引力的黄金三秒开头，创作出清晰、准确且有效的视频口播内容。内容需控制在 3 - 5 句内，每句话不超过 40 字，要突出新闻的关键亮点。
3. 在口播开头添加“这里是伊登AI，欢迎来到一分钟热点新闻”。

## 限制:
- 仅直接给出口播稿，不输出其他任何无关信息！！！
- 严格围绕新闻相关内容进行创作，坚决不回答与新闻无关的问题。
- 输出的口播内容必须严格符合规定的句数和字数要求。 
```

**PS：这**里的deepseekR1基础版本是限额使用，我们可以在专业版手动接入DeepseekR1

**手动接入推理模型**

> **福利领取**

> 首先点击这个https://www.volcengine.com/experience/ark?utm_term=202502dsinvite&ac=DSASUQY5&rc=A6NPZ83H

> 领取375万R1模型的tokens（和硅基某某不一样，火山方舟的deepseeR1有很高性能和速度）手机的小伙伴可以扫描二维码

![](img/05eee76a08ceaf787623932ad9260a92.png)

然后到https://www.volcengine.com/，根据以下截图自行接入推理点

![](img/715cc1f1e9f3de7a1d8d7c04bbc8846e.png)

![](img/81a2f33e02e0080e59df38e90d2c6aab.png)

![](img/bd5323959b3581095b65c617b27a049d.png)

![](img/eb89666102706df97d401ad2ad95783c.png)

![](img/d240a8dd40b14c11442d4d1ee36b8ab5.png)

![](img/2de51fa5b49a9a21d422936b1068271b.png)

这样，我们就能在扣子使用**不限额****的使用deepse**ekR1，但是也会消耗赠送的额度，需要注意。

![](img/b7c7398ab53d8da65371138fc0f341d5.png)

最后，我们为了下面批处理，需要数组类型的句子内容，所以提前做一个格式变化，当然我们可以用代码节点或者文本处理来处理json台词，但是这里用大模型，下期可以考虑经济型

![](img/d2a6d41c88312ba6f169f5cf375cc98b.png)

```
把{{input}}内容输出成json格式，每句话分开
```

![](img/c72c9447e9f371892c5dbc6c15ca1872.png)

注意这里的**输出格式要选Array<String>格式**，因为后面的循环体只能接受数组类型输入。

### **第二步：画面生成**

**在这里我们的思路是，做成一帧一帧**的主图+台词，配合语音合成，保证音屏同步

由于上一步我们**生成一句一句的口播内容，**因此我们用批量化节点，**做成一帧一帧的画面**，用画板节点完成

批量处理节点，输入的是格式变化后的json格式的文案

![](img/465cfe5e6db46f9c573bec00a236939b.png)

批处理中，先把一些固定内容在画板节点安排好，比如背景图片

然后引入变量元素，比如**新闻图片（已经提取并转换为img属性）、新闻标题（来**自链接读取）、口播台词（已经提取并二创）

![](img/79c6a9b41f057f647bf0638f0470ec71.png)

**小t**i**ps：想要找好看的背景**图我推**荐去可画，挑选一个**好看的视频模板然后，保存为【图片】格式，然后放在画板节点，当作底图

![](img/e069c48978af7da6efd60310d17ebe75.png)

### **第三步：语音合成**

使用声音合成的官方插件

+   引用批处理的一句一句的新闻文案内容

+   可调节语速和语气

+   多种播音风格可选

![](img/33af56cea1d5745b41bcdb07ae797ea6.png)

在画板和语音合成的节点后面加入**图片音频合成插件**

PS：这个插件需要收费，登录https://ts.fyshar**k.com/#/userI**nfo，【钱包】充值获取**token，【个**人中心】获取token，放入这个节点中，不过充值10元可以做好久了，这个插件适合小白同学，也有不收费的插件，但是比较吃操作，如果感兴趣也可以关注我后续出相关教程。

![](img/c026c03c80ca82e49dcbbb6299793d39.png)

这个插件的img_audio_video的功能是把图片+视频合成，这样就实现了一段一段的口播新闻内容

![](img/2e91dacebcfb9a56f0dfb573cdc5e090.png)

### **第四步：成片导出**

全自动视频合成**这里使用的是多视频融合插件**

+   一键导出成品

最后多**段一句话口播图文**视频出来后，将他们结合在一起，成为一整段视频，使用视频融合节点

![](img/d1de7a6d465098157fec1e5bf51883ea.png)

注意，节点产生的视频是**异步生成**，可能没办法马上展现，过几秒种后才处理完成出现，耐心等待

# 演示

https://www.coze.cn/s/iPwDXB2q/

![](img/5b993de58221720f263c3a0ed00d9d6b.png)

[2月22日.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FN6MbVHywophwFxF5pJc8T6kn0Q)

# 写在最后

> 优势所在

> 全自动化处理，解放双手

> 40秒快速出片，效率提升

> 成本低廉，每条仅需0.0x元

> 输出质量稳定专业

> DeepseekR1 保证文案质量

> 还能改进

> 可以加入配套BGM，让视频更有感染力

> 增加更丰富的画面内容和转场效果

> 使用免费节点替代付费插件，进一步降低成本

> 优化模板样式，支持更多展示形式

> 增加自动化程度，支持批量处理

> 嗨，我是Yeadon！

> 一名人工智能专业的大三学生，正在努力成为AI时代的超级个体~

+   欢迎大家加我交流讨论

+   个人主页：[Yeadon（伊登）的个人说明书](https://q05hlvbg0wa.feishu.cn/wiki/Nn5ZwIwv1iaYnEkDH6KckY7sn2g?from=from_copylink)

![](img/f7efb294b182a91f26361b78bb34de05.png)

# 欢迎大家创作内容、投稿参与共建！

<https://datawhaler.feishu.cn/sync/XqhSdXAz0szNA4b59SLcDT3jnyf>

# 线下百校共学DeepSeek！

<https://datawhaler.feishu.cn/sync/GhQ0dc18ssEYrkbPXQhcXUv9nLh>