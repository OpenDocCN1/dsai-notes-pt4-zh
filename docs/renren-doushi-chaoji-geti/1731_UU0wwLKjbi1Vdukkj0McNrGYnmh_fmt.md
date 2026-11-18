
# 2023-07-08--0代码微调大模型火了，只需5步，成本低至150块

> 来源：[https://xuqiwei1986.feishu.cn/docx/K8DGdCniBoerHbxJuKjcudJ7nnh](https://xuqiwei1986.feishu.cn/docx/K8DGdCniBoerHbxJuKjcudJ7nnh)

`https://monsterapi.ai/`

0代码微调一个大模型，成本不到20美元（人民币约144元）？

过程也非常简单，**只需5步**。

如LLaMA、GPT、StableLM等爆火的开源生成式模型，都能搞定。

![](img/c4ec2eaba30819444bb287d39ba72dba.png)

这就是 Monster API，一个最新火起来的API平台。

![](img/57d2647cf0bb22ac0c3130e2f693779a.png)

有人觉得这个开源领域的新工作，能改写AI开发的游戏规则，加速AI应用的速度。

还有人兴奋提问，之后会接入GPT-3/GPT-4吗？

所以，它具体是怎么实现的？

## 五步零代码搞定

简单理解，Monster API就是将微调步骤尽可能简易化，让开发者不用再手动进行一系列设置，同时也提供便宜的GPU资源及内存优化。

具体过程如下：

**第一步**，选择一个模型进行微调。

比如LLaMA-7B、GPT-J-6B、StableLM-7B等，Monster API提供了至少10个基础大模型。

**第二步**，选择或创建微调任务。比如指令微调、文本分类等，或者自定义任务。

![](img/ed0dce2e0365ff69636d5927391cb805.png)

**第三步**，选择一个HuggingFace数据集。

Monster API可以无缝集成HuggingFace数据集，可选择范围很大；还可以根据任务类型推荐数据集。

并且不用自己手动操作，就能自动设置好格式。

![](img/f7a358fe7376a436d87704ea2a75a358.png)

**第四步**，设置超参数。

![](img/5d06dcbc8f9c91c8e43fd43bf35097fc.png)

**第五步**，检查和提交。

在设置好如上所有步骤后，确定没有错误就可以提交了。

Monster API表示可以通过WandB上的日志来监测任务。

在其博客中写道，用DataBricks Dolly 15k微调LLaMA-7B完成3个epouch，成本仅需不到20美元（折合人民币约144元）。

官网显示，注册用户后会赠送2500积分。会员分为三档，收费分别是每月9美元/29美元/39美元。

除了微调以外，Monster API还提供各种生成式AI的API接口，并表示成本比其他方案低80%。

![](img/59d461854b227fcb0bef9d2a7e021833.png)

## 背后公司已获110万美元融资

消息显示，Monster API背后公司拿下了**110万美元**的种子前资金（pre-seed funding）。

这家AI初创公司给自己的定位是要做“GPU领域的爱彼迎”，把全世界分散的GPU资源实现灵活调度，让开发者们能够以更低的价格使用。

创始人是两兄弟Gaurav Vij和Saurabh Vij。

其中Gaurav Vij还创立了一家CV公司，正是因为CV公司需要面临巨大的云计算资本，才启发了他们要做这样一个平台。

Saurabh Vij之前是欧洲核研究中心的粒子物理学家，同时也研究分布式计算。

兄弟俩表示在经过多轮技术迭代后，他们优化了消费级GPU在机器学习任务上的性能，使得运行Whisper AI模型的成本相较于AWS平台降低了90%，所以他们就想着为什么不利用这个方法来帮助数以万计的开发者们。

同时他们透露公司的一个客户使用他们的分散的GPU计算资源，已经节省了30万美元。

参考链接：

[1] `https://blog.monsterapi.ai/no-code-fine-tuning-llm/`

[2] `https://www.enterpriseai.news/2023/06/09/monster-api-launches-the-airbnb-of-gpus-with-1-1m-pre-seed/`
