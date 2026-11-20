
# RouteLLM：并非所有的任务都需要GPT-4

> 来源：`https://agijuejin.feishu.cn/docx/DCbCdHeFOosXq3xTtKPcWZYXnSd`
>
> 作者：PaperAgent
>
> 原文：`https://mp.weixin.qq.com/s/iEil5Me8PAJN3ZhwuxsFaw`

## 背景与挑战

LLM 在一系列任务中都表现出了卓越的能力，但从图1中的性能与成本图可以看出，它们的成本和能力存在很大差异。从广义上讲，能力更强的模型往往比能力较弱的模型更昂贵。这导致在现实世界中部署LLM时出现两难境地：将所有查询路由到最大、能力最强的模型可以获得最高质量的响应，但成本可能很高，而将查询路由到较小的模型可以节省成本，但可能会导致响应质量较低。

图1：各种LLM的性能与成本图。性能由Elo在Chatbot Arena上测量，每百万token的成本假设为1:1的输入/输出比。通过两个模型之间的路由，理想情况下实现了比任何一个模型都更好的性能：成本比。

![](img/6f73937ae1f1aa7b194046eb6da4b5ff.png)

## RouteLLM 框架

LLM最权威评估榜单维护者 lmsys 开源 RouteLLM（LLM路由），提供了一种解决方案，其中每个查询首先由一个系统处理，该系统决定将其路由到哪个 LLM。理想情况下，所有可以由较弱模型处理的查询都应路由到这些模型，而所有其他查询则路由到较强模型，以最大限度地降低成本，同时保持响应质量。

RouteLLM 是一个基于偏好数据的LLM路由原则框架。将LLM路由问题形式化，并探索增强技术以提高路由器性能。使用Chatbot Arena 的公共数据训练了四种不同的路由器，并证明它们可以在不影响质量的情况下显著降低成本，与仅使用GPT-4相比，MT Bench上的成本降低了85%以上，MMLU上的成本降低了 45%，GSM8K上的成本降低了35%，同时仍能达到GPT-4的95%的性能。

+ 使用 Chatbot Arena数据和数据增强的混合方式训练了四个路由器：
+ 相似度加权 (SW) 排名路由器，根据相似度执行“加权 Elo 计算”
+ 矩阵分解模型，用于学习评分函数，以评估模型回答提示的能力
+ BERT 分类器可以预测哪个模型可以提供更好的响应
+ 因果 LLM 分类器还可以预测哪个模型可以提供更好的响应

## 实验评估

在三个流行的基准上评估了这些路由器：MT Bench、MMLU和GSM8K，为了进行评估，在GPT-4 Turbo作为强模型和Mixtral 8x7B作为弱模型之间进行路由。使用之前的随机路由器作为基线。

+ 矩阵分解和SW排名都具有出色的性能。值得注意的是，矩阵分解能够使用 26%的GPT-4调用实现95%的GPT-4性能，与随机基线相比便宜了约 48%。
+ 使用LLM Judge增强Arena数据可显著改善所有路由器的性能。在使用此增强数据集进行训练时，矩阵分解再次成为性能最佳的路由器，实现95%GPT-4性能所需的GPT-4调用次数进一步减半，占总调用次数的14%，比随机基线便宜75%。

图2：仅在Arena数据上训练的MT Bench上的路由器性能（左），在使用LLM评判员增强的Arena数据上训练的路由器性能（右）。

![](img/64f35ad944a1179b7a7a2f141ca3b05f.png)

+ 当仅在Arena数据集上进行训练时，所有路由器在近乎随机的水平上表现不佳，将其归因于大多数MMLU问题超出了分布范围。但是，使用来自MMLU验证拆分的标准标签数据扩充训练数据集可显著提高所有路由器的性能，表现最佳的因果 LLM路由器现在只需要54%的GPT-4调用即可实现95%的GPT-4性能，比随机基线便宜14%。重要的是，这个大约 1500 个样本的增强数据集占整体训练数据的不到2%，证明了即使样本数量很少，数据增强也是有效的。

图3：MMLU上的路由器性能（左）仅在Arena数据上进行训练（右），该数据是在使用来自MMLU验证分割的标准标签数据增强的Arena数据上进行训练的。

![](img/2801281e7d2530fefa4e722cbfd099f9.png)

## Demo 与体验

lmsys还构建了一个小demo。例如，编码问题被路由到GPT-4-1106，博客写作被路由到 Mixtral-8x7b。体验地址：

`https://0c83f754b05f4a2208.gradio.live/`

![](img/2b8e74e2294de0f8252ca5b519d568af.png)

![](img/0984a97638df83d236d8a44ef893f1c6.png)

```
https://github.com/lm-sys/RouteLLM
https://arxiv.org/abs/2406.18665
RouteLLM: Learning to Route LLMs with Preference Data
```

## 推荐阅读

+ 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO
+ 2024：ToB、Agent、多模态
+ [RAG全景图：从RAG启蒙到高级RAG之36技，再到终章Agentic RAG！](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)
+ [Agent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)

---

欢迎关注我的👸号“PaperAgent”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
