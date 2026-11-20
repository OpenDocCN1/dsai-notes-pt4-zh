
# Al 行业资讯3.3

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/F380dgtYIoMhL4xSkXNcleJVnYf](https://qqs7y1hozd1.feishu.cn/docx/F380dgtYIoMhL4xSkXNcleJVnYf)

+   DeepSeek一周开源八大项目，助力AI发展
+   Kimi最新曝光模型k1.6登顶LiveCodeBench榜首
+   OpenAI发布GPT-4.5
+   腾讯混元发布快思考模型Turbo S，实现秒回与成本优化
+   智元机器人推出大型仿真框架AgiBot Digital World及开源数据集
+   AI搜索引擎Perplexity推出AI浏览器comet
+   AWS推出新型量子计算芯片Ocelot
+   百度文心大模型4.5即将上线

## DeepSeek一周开源八大项目，助力AI发展

本周，DeepSeek在GitHub上连续开源八大项目，涵盖计算加速、专家模型通信、矩阵计算优化、流水线并行革新、数据基础设施支撑等，全面助力AI发展。

### 下面是DeepSeek所开源的项目概览：

+   `FlashMLA`：为Hopper GPU优化的高效MLA解码内核，显著提升模型处理速度，降低推理成本。
+   `DeepEP`：专为混合专家（MoE）模型设计的通信库，提升训练和推理效率。
+   `DeepGEMM`：轻量级`FP8`矩阵乘法库，简化设计，性能提升显著。
+   `DualPipe`：双向流水线并行算法，减少等待时间，提升训练效率。
+   `EPLB`：解决专家并行中的负载均衡问题，确保工作量均衡合理。
+   Profiling Data：公开性能分析数据，助力开发者深入了解系统优化。
+   `3FS`：高性能分布式文件系统，支持大规模数据读取。
+   `smallpond`：轻量级数据处理框架，高效完成大规模数据排序。

此外，DeepSeek还公开了`V3/R1`推理系统的设计细节，展示了其在实际生产环境中的应用价值。

![](img/974534e90abd45b64ba09973b3e7efb1.png)

## Kimi最新曝光模型k1.6登顶LiveCodeBench榜首

近日，月之暗面旗下的Kimi的最新模型k1.6被曝光，k1.6在LiveCodeBench的代码生成基准测试中取得了优异成绩。k1.6-high版本在多项测试中超越了OpenAI的GPT o3-mini、o1以及通义QwQ-Max等多款模型，登上了测试榜榜首。

月之暗面研究员Flood Sung在其个人社交账号上表示，k1.6是一个基于k1.5持续训练代码能力的特定版本，目前仍在不断训练并提升性能。这一成果展示了Kimi在代码生成领域的强大能力和持续进步。

月之暗面创始人张予彤在朋友圈表示，尽管k1.6模型尚未正式发布，但看到其在LiveCodeBench测试榜上的优异成绩感到非常开心。她强调，k1.6目前仍在持续训练和优化中，团队对模型的未来发展充满期待。

![](img/0391a5464269b1a9fc5b2eb8abb978a2.png)

## OpenAI发布GPT-4.5

2月28日，OpenAI发布了最新模型GPT-4.5，作为“研究预览”版本，目前仅对Pro订阅用户（每月200美元）和拥有API Key的开发者开放。尽管OpenAI称GPT-4.5为“迄今知识最丰富的模型”，但其高昂的成本和性能表现引发了广泛争议。

GPT-4.5在写作能力、世界知识和个性化优化方面有所增强，用户与模型互动的体验更加自然。然而，OpenAI也承认，GPT-4.5并非前沿模型，其性能可能不及o1或o3-mini。此外，GPT-4.5的API调用价格非常昂贵：输入100万个token需要75美元，输出100万个token需要150美元，远高于o1和GPT-4o。

许多用户对GPT-4.5的表现感到失望。在SimpleQA任务中，GPT-4.5的幻觉率为37.1%，虽然相比GPT-4o（61.8%）和o3-mini（80.3%）有所改善，但仍略逊于o1（44%）。在编程任务的基准测试中，GPT-4.5的表现与o3-mini相当，但大幅落后于DeepSeek V3和Sonnet 3.7。

行业专家对GPT-4.5的评价也较为负面。Andrej Karpathy指出，尽管GPT-4.5在训练计算量上是GPT-4的10倍，但提升幅度并不显著，尤其是在数学和编程等依赖推理能力的领域。Eli Lifland认为，GPT-4.5的成本比GPT-4o高出15倍，但性能提升并不匹配其高昂的成本。

OpenAI研究科学家Rapha Gontijo Lopes承认GPT-4.5是一个非常庞大且计算资源密集的模型，比GPT-4o更昂贵，且并不能替代GPT-4o。OpenAI正在评估是否长期在API中提供GPT-4.5，以平衡当前能力支持与未来模型的构建。

![](img/43ac1ee90dfa152e97ea0ecc7cdc80c2.png)

## 腾讯混元发布快思考模型Turbo S，实现秒回与成本优化

2月27日，腾讯混元正式发布了自研的快思考模型Turbo S。与需要“想一下再回复”的慢思考模型（如DeepSeek R1、混元T1）不同，Turbo S能够实现“秒回”，吐字速度提升一倍，首字时延降低44%，在知识、数理、创作等方面也有突出表现。通过模型架构创新，Turbo S的部署成本大幅下降。

在多个公开Benchmark上，混元Turbo S在知识、数学、推理等多个领域展现出与DeepSeek V3、GPT-4o、Claude3.5等业界领先模型相当的效果。通过长短思维链融合，Turbo S在保持文科类问题快思考体验的同时，显著改进了理科推理能力。架构方面，采用`Hybrid-Mamba-Transformer`融合模式，有效降低了传统Transformer结构的计算复杂度，减少了`KV-Cache`缓存占用，实现训练和推理成本的下降。

目前，混元Turbo S已在腾讯云官网上架，开发者和企业用户可以通过API调用。同时，该模型将逐步在腾讯元宝灰度上线。混元还表示，正式版的混元T1模型API也即将上线，对外提供接入服务

![](img/040dac84079ff3257b4f0eba5ba8e8e3.png)

## 智元机器人推出大型仿真框架AgiBot Digital World及开源数据集

2月25日，智元机器人宣布推出自主研发的大型仿真框架「AgiBot Digital World」。该框架专为机器人操作技能研究与应用设计，集成了海量逼真的三维资产、多样化的专家轨迹生成机制和全面的模型评估工具。通过高保真模拟训练场景和全链自动化数据生成，AgiBot Digital World可提供一系列具身仿真服务。

智元机器人还开源上线了海量规模的仿真数据集AgiBot Digital World Dataset，涵盖5大类场景、180+品类具体物品、9种常见材质、12种核心技能，具有质量高、泛化快、任务多样、应用灵活的特点。

目前，AgiBot Digital World已上架智元官网，仿真数据集也已上架HuggingFace。

![](img/21b211d76f2f25c7ab709814fd728998.png)

## AI搜索引擎Perplexity推出AI浏览器comet

近日，AI搜索引擎Perplexity宣布将推出AI浏览器，命名为「comet」。据官方介绍，comet将基于Agentic（智能体）技术打造，但目前尚未透露更多细节。目前，comet已开放报名预约。

近期，Perplexity还推出了「Deep Research（深度研究）」功能，主打深度检索和专业输出。在Humanity's Last Exam测试中，该功能获得了21.1%的准确率，远高于Gemini Thinking、o3-mini、o1、DeepSeek-R1等其他领先模型。

目前，Deep Research功能已全量推送，为免费用户提供每天五次的试用机会。

![](img/69bcb1ea48a4eeac6f64405e243ffc06.png)

## AWS推出新型量子计算芯片Ocelot

近日，亚马逊网络服务公司（AWS）宣布推出新型量子计算芯片Ocelot，该芯片由加州理工学院AWS量子计算中心团队开发，采用新架构设计，内置纠错功能。

Ocelot首次将「猫量子比特」技术与其他量子纠错组件结合到一块芯片上，取得构建容错量子计算机的突破。AWS量子硬件总监Oskar Painter表示，因纠错资源大幅减少，基于Ocelot架构的量子芯片成本可能只有现有方案的五分之一，有望最多提前五年制造出实用量子计算机。

目前，相关成果已刊登在《自然》杂志上。

![](img/5e651ae47fc5118cbade0482560ac83a.png)

## 百度文心大模型4.5即将上线

本周，百度宣布旗下文心大模型4.5将于3月16日正式上线。文心大模型4.5在基础模型能力上有大幅提升，具备原生多模态和深度思考能力。

百度表示，文心大模型4.5将是百度迄今为止最好的模型。

此外，百度还宣布文心大模型4.5将于6月30日正式开源。同时，文心一言将于4月1日0时起全面免费，所有PC端和APP端用户均可体验文心系列最新模型。

![](img/dbfe32a03ba2659f29f5b054cf81cc3b.png)
