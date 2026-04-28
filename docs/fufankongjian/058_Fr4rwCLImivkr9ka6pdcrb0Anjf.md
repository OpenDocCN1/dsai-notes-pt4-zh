# DeepSeek R1本地知识库问答快速实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Ic3QdSGR3oy4IixqK6DcLdOJnVh](https://kq4b3vgg5b.feishu.cn/docx/Ic3QdSGR3oy4IixqK6DcLdOJnVh)

# 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/2409d36f2e300e6323109c388d6b8916.png)

![](img/8214c5bc26bae3e8e24c3c308ac604ff.png)

此外，公开课全套学习资料，已上传至网盘（https://pan.baidu.com/s/15UyjwfDNouCFyra7Dp3JHw?pwd=29tt  ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## **一** **、** **主** **流** **大** **模** **型** **W** **e** **b** **对** **话** **框** **架** **介** **绍**

本节公开课，我们来详细介绍如何借助DeepSeek  R1进行本地知识库问答。

此前公开课中，我们已经详细讨论了DeepSeek R1从原版模型到蒸馏模型的各系列模型本地安装部 署方法，以及借助ollama、VLLM、LMDeploy   等主流推理框架进行调用等方法，详情见：https://www.bilibili.com/video/BV19kFoe6Ef7/

![](img/6a6ec03720ab562ca14fe949958cf9c5.png)

本节公开课，我们继续探讨如何借助DeepSeek  R1进行本地知识库问答。

关于本地知识库问答，目前有非常多实现方法，主流的本地知识库问答开源框架如下：

### 【个人】多端&多模型聊天框架：ChatBox

项目展示：

![](img/8261f56e301c82d765e832edc589ef15.png)

GitHub地址：https://github.com/Bin-Huang/chatbox

### **【个人】轻量级便捷RAG** 推理框架：AnythingLLM

**o 项目展示：**

![](img/dc6623815050877a7519aae6de377dd6.png)

![](img/4a112638f22d638da0e270389721b3df.png)

o  项目官网：https://anythingllm.com/

### 【企业级】RAG&GraphRAG   知识库对话框架：kotaemon

o  项目展示：

![](img/5aefabbb5ccefb18bc4cc171186c59d6.png)

![](img/227e7650100cc51b6a39d8a3d17a2543.png)

Github主页地址https:/github.com/Cinnamon/kotaemon

### 【企业级】RAG&Agent  多功能综合前端框架：Open-WebUI

项目展示：

![](img/d3d94ba7b31aff4bdcc312507e1af2e4.png)

![](img/b7e719653c75b71e2dd0c2124d15b9f3.png)

GitHub项目地址：https://github.com/open-webui/open-webui

### 【企业级】多模态交互框架：lobe-chat

项目展示：

![](img/b0bf9e4520bddc2257fbe0a279c28f61.png)

![](img/f864e92fd552e8dfccddb2566eb4a3ca.png)

GitHub项目地址：https://github.com/lobehub/lobe-chat

本节公开课将以AnythingLLM为例介绍DeepSeek R1本地知识库问答实现全流程，而对于更加复杂的框 架，如Open-WebUI、Kotaemon  等，我们提供了完整的完整的参考课件资料，与本节公开课课件、代 码、软件等保存在同一网盘内，链接: https://pan.baidu.com/s/1SdOUanl5eLTb8bqNbsfQFQ 提取码: 76ym

![](img/12be6e8ee636be9540cfe9e607570bd8.png)

![](img/da1e835809980d3f030bd94801dba189.png)

![](img/ca684cf51d2697cd312c9c9627ed24e9.png)

# 二**、O**l**lama+DeepSeek** R1 Distill模型本地部署

●模型选择与硬件配置说明

本节公开课以Windows 系统为例进行演示，MacOS、Linux 系统实现方式也类似。同时，公开课重 点介绍完全本地部署实现方案，并采用DeepSeek  R1蒸馏模型进行演示，若硬件条件允许，也可考虑部 署原版DeepSeek R1模型。各模型运行所需显存如下：

| **DeepSeek** **R1模型组** | **模型推理性能** | **半精度运行所需显存** | **全精度运行所需显存** |
| --- | --- | --- | --- |
| DeepSeek-R1-1.5B(Distill) | **GPT4o级** | **1.1G** | **4G** |
| DeepSeek-R1-7B(Distill) | **超越GPT40** | **4.7G** | **14G** |
| DeepSeek-R1-8B(Distill) | **超越GPT4o** | **4.9G** | **14G** |
| DeepSeek-R1-14B(Distill) | **超越GPT4o** | **9G** | **24G** |
| DeepSeek-R1-32B(Distill) | **o1** **mini级** | **20G** | **55G** |
| DeepSeek-R1-70B(Distill) | **o1** **mini级** | **43G** | **120G** |
| DeepSeek-R1-671B | **o1级** | **404G** | **1000G** |

而关于如何使用DeepSeek R1在线API实现相关功能，详见Part 4部分内容介绍。

●模型本地调度框架选取

同其他主流大模型对话Web框架类似，AnythingLLM 同时支持调用在线模型API、或本地模型进行推理， 并且AnythingLLM 本身是一款集成度非常高的聊天框架，我们只需要借助可视化页面手动点击选择在线 模型AP 或者本地推理框架即可，例如一些主流API厂商：

![](img/73025b613746ab3477b769c570c60d99.png)

**或者一些主流的本地推理框架。**

![](img/6a5014b1bf5410ea33b0aa0d3a77b1ae.png)

本节公开课我们将以Ollama 为例进行演示。

# **Step 1.安装ollama**

首先需要先安装ollama  地址，下载地址： https://ollama.com/

![](img/8c905d49fdb2082d1d984b021bf52cb2.png)

选择对应的操作系统进行安装即可：

![](img/8b69864e701de86b0f88fa2b4c367a04.png)

Ollama 下载存在网络限制，对应Windows、MacOS    软件安装包开在课件网盘中领取，链接: https://pan.baidu.com/s/1SdOUanl5eLTb8bqNbsfQFQ 提取码: 76ym

![](img/befcffe488dd70a7f126feed2608c620.png)

下载后即可开始安装：

![](img/8340f7b5bcba260a2bb0279eed284b10.png)

![](img/1885ee9c758b10da63a37c705a3fddb6.png)

而当Ollama 安装结束后，会自动在后台开启Ollama 进 程 ，Windows  系统下可以按住Win+r 键，输入cmd 开启命令行，并在命令行中查看Ollama 运行情况：

![](img/14222248f4572836447ae48c3b9cccaa.png)

例如，输入tasklist   |findstr   ollama查 看Ollama 进程是否存在

![](img/b235e82d4ccc16bcf979c71d85d6a237.png)

如此则说明Ollama 已启动。若未启动，可输入ollama start启 动Ollama。

# Step 2.下载DeepSeek R1模型

接下来即可进一步下载DeepSeek    R1模型了，这里我们可以直接使用ollama pull命令自动下载与 注册，也可以手动下载模型的GGUF 文件，然后手动完成Ollama 模型注册，再进行调用。

这里我们考虑使用ollama   pull方[法进行模型权重下载，而若网络条件不允许，也可以参](https://www.bilibili.com/video/BV19kFoe6Ef7/)考我的另 一个视频：《DeepSeek  R1本地部署流程详解》https://www.bilibili.com/video/BV19kFoe6Ef7/        , 从 魔 搭社区上手动下载模型GGUF 权重，并完成Ollama 注册与调用。

![](img/2c7b23601d78ee9a9e98e17d4497bed0.png)

**首先输入01lama** **list查看当前模型列表：**

![](img/ab44f217cc3b518d4d1b109abf32de06.png)

然 后 回 到ollama deepseek  r1模 型 列[ 表 ， 查 看 模 型 组 情 况](https://ollama.com/librarv/deepseek-r1) ：https://ollama.com/librarv/deepseek-r1

![](img/a3986c6e7969688fe2ffb05dec78d073.png)

![](img/3a2c1adb0fae56021eb880a1f435f858.png)

**其中不同尺寸模型运行所需占用显存**大**小如上所示。需**要**注**意**的**是**，**ollama 只支持运行Q4 K M 模 型，相当于是4bit量化后的模型，因此每个模型运行所需占用显存并不大。

**接下来使用如下命令下载模型，这里以14b模型为例，**下载其他模型的话只需要修改模型名称即可， 例如修改为deepseek-r1:7b,     则代表下载7b模型：

```
ollama pull deepseek-r1:14b
```

![](img/1dd359dc5e04d4d512aab6fb371b5cc4.png)

稍等片刻即可完成下载。下载完成后即可使用0llama   list命令查看当前模型是否完成下载与注册：

![](img/8ac85679a49203645674d65fd2133014.png)

接下来即可调用DeepSeek R1 Distill 14B模型进行本地知识库问答了。

# **三** **、** AnythingLLM安装与使用

AnythingLLM  是一款集成度非常高、且功能非常稳定的大模型对话前端产品，虽无法进行二次开 发，但功能实现方面非常便捷，可以直接从AnythingLLM  官网进行下载： https://anvthingllm.com/

![](img/f782a283c5295c2f70241f41d4a40998.png)

选择对应的操作系统版本下载安装即可开始安装：

![](img/9f780b3a7afa46cb6f8365b12ea8548c.png)

安装完成后，会自动启动AnythingLLM, 并进入设置页面

![](img/c2c77587d8091d09bbc79a58eac96c4d.png)

点击Get   started,则进入到选择LLM页面：

![](img/f51ff4cd75f5e2c55e26ffc141afa28f.png)

这里我们选择借助Ollama 来调用本地的14B模 型 。这 里需要注意的是，AnythingLLM 会自动检测Ollama  进程是否存在(也就是Ollama 是否启动),以及现在Ollama 中已经注册了哪些模型。因此在进入到上面 这个设置也面前，需要确保Ollama 已经启动，并且已完成了相关模型注册。选择好14B模型之后即可点  击进入下一步：

![](img/22fc3acdb9c5fd0d6b641521e2deb4b0.png)

该页面是进行模型配置说明，当前的模型配置是：

●对话模型：负责完成对话任务，由Ollama 调 度DeepSeek R1 Distill 14B来完成对话；

●Embedding   模型：负责进行词向量化的模型，由AnythingLLM  官方提供；

●词向量数据库：用于存储词向量化之后的对象，默认为LancelotDB;

对于任何一个本地知识库问答的过程，都需要经历文本切割、词向量化、词向量存储、词向量检索与问 答几个基本环节，因此至少需要Chat 模 型 、Embedding   模型以及词向量数据库作为基础组件。对于RAG 的基本原理回顾如下：

![](img/b219b4d3f0314d9bfbf24e7017d188f1.png)

在浏览了当前模型配置后，接下来即将进入AnythingLLM注册页面，这里可以直接跳过：

![](img/9d553f086427fece2ecfe2366ee08bda.png)

然后需要创建一个workspace,  相当于是ChatGPT 或者其他开发项目的一个Project,用于对对话进行分 类：

![](img/fca50f3a4277e8fbd85c495ffb84d708.png)

当我们进入到如下页面，则说明安装成功：

![](img/dc8affcb50cdd29cc31187283a791bda.png)

接下来即可尝试进行对话了。AnythingLLM  对于DeepSeek    R1系列模型的支持是非常友好的，甚至会区

分思考过程和回答过程：

![](img/b0437690a54ba940809a1fa410047d6b.png)

并且支持多轮对话：

![](img/674802c0168c89855a71292bfe9c84d8.png)

# 四、借助AnythingLLM进行本地知识库问答

AnythingLLM 是一款集成度非常高、且运行稳定的RAG+ 前端对话框架，我们可以非常便捷的为每次 对话设置知识库。主要方法有以下两种：

●在单次问答中设置背景文本

例如，我们可以直接在对话框下方点击回形针按钮，即可上传文本并进行对话，整个过程类似 ChatGPT的功能实现：

注：这里所谓上传文本，并不是把文本上传到远程服务器，而是将文本进行切分、Embedding 后 后上传到本地的LanceDB词向量数据库中。整个过程全部都在本地完成。

![](img/37f565557d3ae2623320dcc855e7417f.png)

**上** **传** **文** **本** **：**

![](img/82d324cf24a6a0911b5b6a1ffdb0cf78.png)

**等** **待** **文** **本** **切** **分** **与** **词** **向** **量** **化** **过** **程**

![](img/112cdbbf28d6407eb186ea5dc1ea40c6.png)

**然** **后** **即** **可** **开** **始** **问** **答** **：**

![](img/7b93697d3e062faa06b6b0404366a045.png)

此 时 迷 哦 行 问 答 即 会 根 据 文 档 进 行 回 答 。

●统一知识库管理

此外，若想让整个文本长期存在于当前对话中，则需要借助AnythingLLM 的知识库管理功能。下面这个 icon就是知识库管理图标(UI 需优化):

![](img/bd9bcf4d1b72496527b65eea2c31941c.png)

**点击后即可进入到如下页面：**

![](img/75b9f64dc61b201ada43cef86129710d.png)

这里我们额外上传一份DeepSeek V3技术报告，该报告的内容超出了当前模型只是范畴，适合进行本地 文档问答测试：

![](img/f4a33089a4a64b1e1b2a0285c96251b2.png)

上传后即可在当前文件夹中看到该文件，这里点击Move to Workspace,即可将其列举为备选问答文 档：

![](img/84473735a12092ad900293f671ad1e70.png)

当Move to workspace后，在右侧即可看到这篇文档，然后点击Save and Embed,即可开始进行文档切分和词向量化过程：

![](img/5738a419b00031a9c5043a446cd898e3.png)

**需要稍等片刻：**

![](img/981fe93599d0d5a56116a196527749fc.png)

完成后，再点击图钉按钮，即可将这篇文档设置为当前对话的背景文档：

![](img/48dcabdf0a6ca30032bbc9bd7400cf1c.png)

**然后点击got it**

![](img/c27824460bcd0030491e543ad3f75815.png)

然后即可提问：

![](img/3819cf800725860300b0ee43fcb4b3fc.png)

能够看出，此时回答结果就会根据当前文档进行回答，细节丰满回答流畅。而若不绑定知识库，则模型 并不知道DeepSeek  V3模型：

![](img/fbaa0d7f86e87095b011900614be994d.png)

至此，即完成了DeepSeek R1 Distill 14B模型的本地知识库问答全流程。

# 五、DeepSeek-R1 API接入AnythingLLM   进行本地知识库问答

最后，让我们来看下如何借助DeepSeek R1 API+AnythingLLM来进行本地知识库问答。 首先，新建一个工作区，并命名为RAG Test2

![](img/ae8846976bd39fd7878ceafd0c404976.png)

**然后点击设置按钮：**

![](img/38f672842f028959cb40ca3ddc77371b.png)

设置页面，我们可以灵活设置当前工作区域的名称(或者删除工作区)

![](img/f5214a879270a429889ec0f448de1267.png)

或者修改默认模型，以及system message (系统默认提示):

![](img/c5e523a7dd01bbba3e642e112f306699.png)

**以及进行词向量数据库设置等。**

![](img/cd97b52fb9f8f3af8ccd5cd18dd7bf75.png)

这里我们需要在聊天设置中修改模型为DeepSeek R1 API,先点击聊天设置，然后选择模型：

![](img/d47de769b42ec4c2c9979e9135ad5c1c.png)

然后在模型提供商中选择DeepSeek:

![](img/dd49507b5df7ada1d90bbf42c2ac62cb.png)

然后在弹出的页面输入API-KEY

![](img/161706a8cf5de935337d3e383cc6f243.png)

输入完API-Key后，即可选择对话模型，deepseek-chat 就是DeepSeek  V3,而 deepseek-reasoner则是 DeepSeek  R1。这里我们选择deepseek-reasoner:

![](img/8ec74c050cd07e6a6d80ebc6ee76891c.png)

然后点击Save, 即可返回设置页面：

![](img/eb6fc23632639cae40847a63e47e8716.png)

然后翻到最下面点击Update    workspace,即可完成设置：

![](img/d19df6e8b8e7460457c20c103dae2fd0.png)

然后点击左侧聊天栏，选择default会话即可开始聊天了：

![](img/5d754e715955873575c5b04d337cad4a.png)

如此即可完成对话，而若要进一步设置知识库，可参考Part 4中部分内容，无论是调用在线模型API, 还 是调用本地模型，知识库设置流程都完全一样。