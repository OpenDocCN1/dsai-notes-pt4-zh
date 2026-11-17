
# 《中文大模型基准测评2024年度报告》发布！

> 来源：[https://aigcreative.feishu.cn/docx/U3jhdsOukoDsDkxKXBmcaG0lnxe](https://aigcreative.feishu.cn/docx/U3jhdsOukoDsDkxKXBmcaG0lnxe)
>
> 原文链接：[https://mp.weixin.qq.com/s/XYX85wbYFyKN9wQLDhfRrA](https://mp.weixin.qq.com/s/XYX85wbYFyKN9wQLDhfRrA)
>
> 公众号：SuperCLUE CLUE中文语言理解测评基准

![](img/0844382cacfa9ce9db44363242f602ce.png)

## 背景

自2023年以来，AI大模型在全球范围内掀起了有史以来规模最大的人工智能浪潮。进入2024年，全球大模型竞争态势日益加剧，随着Sora、GPT-4o、o1的发布，国内大模型在2024年进行了波澜壮阔的大模型追逐赛。

![](img/e11bff90faaa5f9d25fea3fd717be76e.png)

中文大模型测评基准SuperCLUE持续对国内外大模型的发展趋势和综合效果进行了实时跟踪，正式发布《中文大模型基准测评2024年度报告》。

报告全文共89页，本文仅展示报告中关键内容，完整内容可点击文章底部【阅读原文】查看高清完整PDF版。

在线完整报告地址（可下载）：`www.cluebenchmarks.com/superclue_2024`

SuperCLUE排行榜地址：`www.superclueai.com`

## 报告关键内容

### 关键内容1：2024年最值得关注的大模型全景图

![](img/14f203d75269fd1e4dd1bc92214af791.png)

### 关键内容2：年度总榜及模型象限

#### 测评介绍

本次年度报告聚焦通用能力测评，由理科、文科和Hard三大维度构成。题目均为原创新题，总量为1325道多轮简答题。

【理科任务】分为计算、逻辑推理、代码测评集；【文科任务】分为语言理解、生成创作、安全测评集；【Hard任务】分为指令遵循、深度推理、Agent测评集。

![](img/47557e6cc6e82a7bc94621236565366f.png)

本次测评数据选取了SuperCLUE-12月测评结果，模型选取了国内外有代表性的42个大模型在12月份的版本。

![](img/828cb48c737fd38d7873620792e783e6.png)

#### 年度总榜

![](img/7bba77f5b90b9207e3a01d9d79608d16.png)

#### 年度模型象限

![](img/3e4a809c61fcd84879ac7083356be7b9.png)

### 关键内容3：性价比区间分布

![](img/5b0413865cf86b8dd4f5fcb797651590.png)

- 国产大模型在性价比（价格+效果）上有较大优势

  国产大模型如DeepSeek-V3、Qwen2.5-72B-Instruct和Qwen2.5-32B-Instruct在性价比上展现出极强的竞争力。在较为高水平的能力基础上能够保持极低的应用成本，在应用落地方面展现出友好的可用性。

- **大部分模型处于中度性价比区间**

  大部分模型为了保持较高的能力水平，价格方面仍处于高位。如GLM-4-Plus、Qwen-Max-latest、Claude 3.5 Sonnet、Grok-2-1212价格方面均处于30元/百万Tokens以上。

- o1等推理模型性价比尚有较大优化空间

  虽然o1和o1-preview展现出很高的能力水平，但在价格方面较其他模型高出数倍。如何降低成本或许成为推理模型广泛应用的前提条件。

### 关键内容4：推理效率区间分布

![](img/0bb7e9eca89403789dc9fd23c23f8894.png)

- 部分国产模型在综合效能上很有竞争力

  国产模型中，DeepSeek-V3和Qwen2.5-32B-Instruct推理速度表现优异，平均每题推理时间在10s以内，同时基准得分在60分以上，符合「高效能区」，展现了极强的应用效能。

- Gemini-2.0-Flash-Exp引领全球大模型应用效能

  海外模型Gemini-2.0-Flash-Exp、Claude 3.5 Sonnet(20241022)、Grok-2-1212和GPT-4o-mini符合「高效能区」，其中Gemini-2.0-Flash-Exp在推理时间和基准得分的综合效能上表现最为出色。GPT-4o-mini在推理速度上表现最优。

- 推理模型在效能上表现有较大优化空间

  以o1-preview为代表的推理模型虽然在基准得分上表现出色，但每题平均推理时间约40s，综合效能符合「低效能区」。推理模型若要有广泛的应用场景，需要在推理速度上着重提升。

### 关键内容5：2024年国内外大模型差距及趋势

![](img/431a23e88e12c984de6931d2ab06e593.png)

- 总体趋势上，国内外第一梯队大模型在中文领域的通用能力差距正在扩大。

  2023年5月至今，国内外大模型能力持续发展。其中GPT系列模型为代表的海外最好模型经过了从GPT-3.5、GPT-4、GPT-4-Turbo、GPT-4o、o1的多个版本的迭代升级。

  国内模型也经历了波澜壮阔的18个月的迭代周期，从2023年5月的0.12%的差距，缩小至2024年8月的1.29%。但随着o1的发布，差距再次拉大到15.05%。

![](img/b2a336c4c5170d117b8e5dfa354a0bec.png)

- **以DeepSeek-V3为代表的国产模型**正极为接近GPT-4o-latest

  在过去2年中，国产代表性模型持续迭代多个版本，DeepSeek-V3、Doubao-pro、GLM-4-Plus、Qwen2.5在中文任务上已经接近GPT-4o。其中DeepSeek-V3表现出色，在12月测评中有超过Claude 3.5 Sonnet的表现。

- o1基于强化学习新范式的推理模型，突破80分拉大国内外顶尖模型差距

  在12月SuperCLUE测评中，国内外主要头部大模型在SuperCLUE基准得分集中在60-70分。o1和o1-preview基于强化学习新范式的推理模型成为突破70分瓶颈的重要技术代表，尤其o1正式版突破了80分大关，展现出较大的领先优势。

### 关键内容6：其他子维度榜单

#### Hard榜单

![](img/bc7c2ca5cc5c8a766f5e02cae05738aa.png)

#### 理科榜单

![](img/d528cdb7965f232039f9b126974d3b86.png)

#### 文科榜单

![](img/ed774e059695a5f523f917a844e1c078.png)

#### 各维度测评国内TOP3

![](img/27414198aefedd86a895f3d8d3e62906.png)

#### 开源模型榜单

![](img/9b31860a32c4823264784b152bfe5ff0.png)

#### 10B以内模型榜单

![](img/c9a947e8be1a9d77f0af4797ffa755e8.png)

#### 5B以内端侧模型榜单

![](img/0dc1833e0943324359271a4b2dab6d72.png)

#### 二级细粒度分数清单

![](img/bb1f499a54505367a7564a1e1ce9edfa.png)

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
