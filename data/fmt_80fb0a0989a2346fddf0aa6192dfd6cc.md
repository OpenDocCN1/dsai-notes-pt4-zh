
# 奇绩大模型日报（10月 22日）

> 来源：[https://miracleplus.feishu.cn/docx/PKxXddMdfoSzhWxnn3McVbp1n1e](https://miracleplus.feishu.cn/docx/PKxXddMdfoSzhWxnn3McVbp1n1e)

## 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，`Lepton AI` 创始成员，曾在阿里云担任高性能 `AI` 平台产品负责人，专注于 `AI` 在多个行业的落地及应用。`Lepton AI` 致力于建立高效可用的 `AI` 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 `GenAI` 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/d94a0231b1dc57aeb5b1d2c9b2d35085.png)

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，`Lepton AI` 创始成员，曾在阿里云担任高性能 `AI` 平台产品负责人，专注于 `AI` 在多个行业的落地及应用。`Lepton AI` 致力于建立高效可用的 `AI` 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 `GenAI` 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/3c7aca6f62b63e757ef99ba50fe20810.png)

## 资讯

### RoboDual：行业首款通用具身操作的双系统协同框架诞生

https://mp.weixin.qq.com/s/LYmjmrw2ay2scSYPg1GDvA

智元机器人与上海人工智能实验室合作推出了首款通用具身操作双系统协同框架——`RoboDual`，旨在融合通才（`Generalist`）的广泛适应性和专才（`Specialist`）的高效精准性，以提升机器人的操作灵活性和准确性。传统具身智能系统中，负责任务规划的“大脑”泛化能力强但实操效率低，而负责运动控制的“小脑”精度高但适应性不足，两者通常难以协同工作。`RoboDual`通过在机器人边缘侧部署该框架，利用“小脑”的低延迟、高精度特点弥补“大脑”的不足，实现高效推理和任务执行。

`RoboDual`框架采用异步控制架构，即在`Generalist`生成一次动作时，`Specialist`能生成多步连续动作，从而在保证鲁棒性和精度的前提下，显著降低了控制延迟。在实验中，`RoboDual`在`NVIDIA RTX 5000 Ada GPU`上实现了15Hz的控制频率，在仿真和真机环境中的泛化和适应能力超越了传统模仿学习方法。相比`OpenVLA`，`RoboDual`在高效任务适配的同时，推理延迟更低，能够以仅5%的数据量实现对新场景和任务的快速适应。

`RoboDual`框架中，`Generalist`部分基于`Prismatic-7B VLM`的`OpenVLA`架构，处理视频帧和指令，自回归生成动作潜变量，并交给`Specialist`模型。`Specialist`使用`Diffusion Transformer (DiT)`架构，结合`Generalist`的动作参考及传感器输入进行多步连续动作预测，确保动态场景中的精确控制。实验表明，`RoboDual`在多个泛化场景下表现优越，提供了更广泛的通用机器人操作解决方案，推动了机器人技术的广泛应用与发展。

![](img/63e395102acfdea3ba200129f0ea49b7.png)

### OpenAI满血版o1剧透：数学代码能力再破天花板，已开启测试评估

https://mp.weixin.q1q.com/s/tauednAxd_VNjsPTr9SloQ

`OpenAI`在官网商业化频道中对即将发布的满血版`o1`模型进行了提前预告。视频显示，满血版`o1`与`GPT-4o`和`o1-preview`（预览版）同场对比，在数学和编码能力上明显领先。据透露，`OpenAI`计划同步发布`GPT`和`o1`两个系列，满血版`o1`正在基准测试中，未来几个月还将为`o1`系列增加网页浏览、文件和图像上传等功能，并支持`ChatGPT`自动选择合适模型。

然而，网友发现一个有趣现象：在`PhD`级别的科学任务中，满血版`o1`竟不敌`o1-preview`。对此，`OpenAI`首席产品官`Kevin Weil`解释，这或许是由于两者的构建方式存在细微差别。目前，`o1`推理模型仍处于改进阶段，未来的测试结果将揭示更详细的性能差异。

![](img/8d4e65043715dc832db4795fdc80b4c0.png)

### 2¹³⁶²⁷⁹⁸⁴¹−1，GPU发现人类已知最大的素数，比第二大多1600万位数字

https://mp.weixin.qq.com/s/QZRE0JU7lSO9rndvNcdetg

近日，人类已知的最大素数被通过`GPU`发现！这个素数是`2¹³⁶²⁷⁹⁸⁴¹−1`，总共有41024320位十进制数字。该发现由英伟达前员工卢克通过开源项目`GIMPS`实现，初步在英伟达`A100`上发现，并经`H100`验证。`2¹³⁶²⁷⁹⁸⁴¹−1`比之前的记录大了1600多万位，是第52个已知的梅森素数，也是`GIMPS`项目发现的第18个。梅森素数以`2P−1`的形式存在，由法国数学家梅森命名，因其高效的素性检验算法备受关注。`GIMPS`项目自1996年成立，作为世界上寿命最长的分布式项目之一，专注于寻找最大梅森素数。卢克的贡献也证明了`GPU`不仅适用于`AI`，还能推动基础数学研究。他的“云超级计算机”使用数千台`GPU`服务器，横跨17个国家，最终发现了这一新素数。虽然梅森素数的实际用途有限，但其探索代表了人类在数学研究领域的追求与荣耀。

![](img/342e9ac6a8560035f1fe2ea04498e889.png)

### 微软一口气发10个商业智能体！内测提高9.4%销售收入，六成500强企业在用Copilot

https://mp.weixin.qq.com/s/NNynk22K0gwmphWauzQKvg

微软CEO纳德拉宣布了`AI`领域的最新进展，一口气推出了10个商业智能体（`Agent`），覆盖销售、服务、财务、供应链等业务场景。这些`Agent`能够自动筛选潜在客户、监控供应链表现、分析客户意图，帮助企业优化业务流程。例如，销售资格`Agent`可以锁定最有价值的商机，供应商通讯`Agent`实时处理供应链延迟问题，客户知识管理`Agent`协助客服快速响应。

这些`Agent`的功能从简单的提示响应到完全自主决策不等，微软表示，目前已有60%的财富500强企业正在使用`Microsoft 365 Copilot`来提高业务效率。例如，`Lumen Technologies`每年预计节省5000万美元，`Honeywell`的生产力提升相当于增加了187名全职员工，`Finastra`将项目时间从七个月缩短到七周。

微软还计划下月启动`Copilot Studio`的公测，允许用户创建自主`Agent`，连接多个数据源并执行复杂任务。例如，`Pets at Home`通过案例整理`Agent`节省了数百万美元，麦肯锡的客户入职`Agent`使流程缩短90%，汤森路透的法律尽职调查`Agent`也大大加速了工作流程。

在安全和数据治理方面，微软为`Agent`提供了全面的保护，确保数据源安全和使用权限管理，`Agent`严格遵循安全与隐私原则。这些`Agent`不仅能为企业创造直接的业务价值，还将重新定义业务流程的自动化和智能化。

![](img/72ca7d93893a0613bddc7b2f0d8d9d28.png)

### 大模型是否有推理能力？DeepMind数月前的论文让AI社区热议

https://mp.weixin.qq.com/s/NdRBGFT6systLwn7p2ER7Q

`DeepMind` 在2023年2月发布了一篇论文《`Grandmaster-Level Chess Without Search`》，展示了一个参数量为2.7亿的`Transformer`模型无需复杂的搜索算法，即可达到特级大师级别的国际象棋水平。该模型通过监督学习从`Stockfish 16`生成的大量棋局数据中学习棋盘动作-值的预测，模型的`Elo`评分达到了2895，优于`AlphaZero`的策略网络和`GPT-3.5-turbo-instruct`。

该论文的结果引发了广泛讨论。支持者认为，这表明`Transformer`不仅是“统计模式识别器”，还具备一定的推理和规划能力。然而，批评者指出该模型的评估方法有局限性，尤其是在“`blitz`”（闪电战）这种快速棋赛环境下，这类比赛依赖直觉而非深度推理，因此不足以证明模型具备真正的推理能力。此外，模型对人类玩家的表现不及其与机器人对战时的得分，这也引发了关于模型泛化能力的争议。

论文中，`DeepMind`通过监督学习将复杂的国际象棋策略蒸馏到前馈`Transformer`中。为了构建训练数据，研究者从`lichess.org`下载了1000万场游戏，并使用`Stockfish 16`为这些游戏的棋盘状态估值。模型使用`FEN`字符串编码棋局状态，并通过标准的`Transformer`架构进行训练。结果表明，随着数据量和模型规模的增加，模型在各种国际象棋谜题和实际比赛中的表现显著提升，表明规模和数据对于模型的推理能力至关重要。

![](img/a5e1844ed37c4c1f8871cd4ff8da6994.png)

### xAI推出API，探索生成式AI市场

https://techcrunch.com/2024/10/21/xai-elon-musks-ai-startup-launches-an-api/

2024年10月，埃隆·马斯克的`AI`初创公司`xAI`正式推出其首个`API`，名为“`grok-beta`”。该`API`提供`xAI`的生成式`AI`模型，按使用量收费，每百万输入`Token`收费5美元，每百万输出`Token`收费15美元。虽然目前`API`的功能相对基础，但支持与外部工具的连接，未来可能会加入视觉模型，具备分析文本和图像的能力。

`xAI`于2023年成立，最初推出的`Grok`模型被集成到`X`（前身为`Twitter`）中，供`X Premium+`用户使用。`Grok`以其“叛逆性格”著称，敢于回答其他`AI`系统拒绝的问题，并且在图像生成和新闻摘要等功能上表现活跃。不过，`Grok`也因其错误频发的摘要和无防护的图像生成功能引发争议。

`xAI`目前正加速追赶`OpenAI`等竞争对手，并已获得包括`Andreessen Horowitz`、`红杉资本`和`富达投资`在内的多家机构的60亿美元投资。马斯克的计划包括使用`X`平台上的数据以及其旗下公司（如`特斯拉`、`SpaceX`等）的数据训练`AI`模型，但这一举措遭到`特斯拉`股东的反对，认为这会分散资源。

`xAI`的下一代`Grok`模型正在田纳西州的孟菲斯数据中心训练，该中心因未经许可使用涡轮机被指责加剧雾霾。公司计划于2025年升级该数据中心，但需得到当地能源监管部门的批准。

![](img/5345e0eb62b63e757ef99ba50fe20810.png)

## 推特

### Willison分享：过去七天内使用Claude Artifacts构建的14个互动式迷你应用

贡献人：

https://x.com/simonw/status/1848372319548240371

我在过去七天内使用`Claude Artifacts`构建了14个互动式迷你应用——以下是一个描述它们的帖子

---

我这周使用`Claude Artifacts`构建的一切

我是`Claude Artifacts`功能的超级粉丝。这个功能允许你通过提示`Claude`创建一个使用`HTML`、`CSS`和`JavaScript`的互动单页应用（`SPA`），然后可以直接在`Claude`界面中查看结果，进一步与`Claude`进行迭代，如果满意的话，还可以复制生成的代码。

我在查看`Claude`的活动导出数据时（我构建了一个`claude-to-sqlite`工具，用来将其转换为`SQLite`，以便在`Datasette`中进行探索），我决定看看我在过去一周内使用了多少次`Artifacts`。结果比我预想的还要多！

能够快速创建一个完整的互动应用——有时是作为一个示例原型，但通常是直接解决问题的工具——这真是一个非常有用的功能。

以下是我在过去七天内使用`Claude Artifacts`做的大部分事情。我几乎为每个项目提供了提示或完整的对话记录。

![](img/c26d90a8a6bac33dc64b534e2ba887d0.png)

### 个性化解释器：只需上传一个PDF，即可收到包含视觉效果和文档中图像的、用你自己声音生成的解释

贡献人：

https://x.com/MisbahSy/status/1848377457574863145

`NotebookLM`很棒，但如果我想使用自己的声音并添加来自`PDF`的视觉效果呢？

🚀 介绍个性化解释器——只需上传一个`PDF`，即可收到包含视觉效果和文档中图像的、用你自己声音生成的解释。

这个视频完全由`AI`生成。🔊

![](img/e0f9f30e8de9aa9f8d382219022e999c.png)

[ssstwitter.com_1729552812467.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZnITbVs3hoyofVxwAbocXZ34nCc)

### IBM和CrewAI宣布合作，与watsonx.ai集成，旨在通过多个AI代理构建和简化工作流程

贡献人：

https://x.com/joaomdmoura/status/1848394061910323325

`IBM`和`CrewAI`从今天起合作，帮助美国及全球的企业大规模采用`AI`代理。

---

宣布新的`CrewAI`与`watsonx.ai`集成，旨在通过多个`AI`代理构建和简化工作流程。

我们非常高兴地分享这一令人兴奋的新合作，将`IBM`的`watsonx.ai™`和`CrewAI`结合在一起。通过结合`IBM`的`AI`专业知识与`CrewAI`独特的协作式代理解决方案，这一集成将彻底改变组织管理复杂工作流程的方式。

![](img/83e142fcfe7f14c981c97be8faafdd2e.png)

### AnthropicAI和OpenAI收入来源对比：前者更像基础设施提供者，而后者更像面向消费者的公司

贡献人：

https://x.com/auchenberg/status/1848427656598970387

根据它们的收入来源，`@AnthropicAI` 作为基础设施提供者出现，而 `@OpenAI` 则更像一家面向消费者的公司。

很期待看看几年后这两家公司会发展到什么程度。

![](img/a3654841909dbb727fa75e01fe2ecaf2.png)

### 谷歌分享CT Foundation：医学影像嵌入工具，可用于快速训练模型

贡献人：

https://x.com/GoogleAI/status/1848437918513434780

宣布`CT Foundation`，这是一个新的医学影像嵌入工具，它接受计算机断层扫描（`CT`）体积作为输入，并返回一个小型且信息丰富的数值嵌入，用于快速训练模型。了解更多并亲自试用 → `https://goo.gle/4dYkClf`

![](img/61a1309af156f4ac7a3ed78715c300e9.png)

[ssstwitter.com_1729586877922.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MiCZbEo63oxOCvxBMkkcZPwRnGd)

## 信号

### Heavy-Tailed Diffusion Models

贡献人：

原文链接：https://arxiv.org/abs/2410.14171

推文链接：[https://x.com/xuyilun2/status](https://x.com/xuyilun2/status/1848490806321230212)/1848490806321230212

研究节点：模型体系- 架构体系

这篇文章提出了一种新的扩散模型框架，用于估计具有重尾分布的数据生成。传统的扩散和流匹配模型使用标准高斯先验，无法捕捉重尾行为。文章通过使用多变量学生`t`分布重新构建扩散框架来解决这个问题，并开发了专门的扰动核，以及基于条件学生`t`分布的去噪后验。受`γ`-散度在重尾分布中的启发，文章推导出了重尾去噪器的训练目标。这个框架通过单一的标量超参数控制尾部生成，使其易于调整以适应多种现实世界分布。文章还介绍了`t-EDM`和`t-Flow`，这是现有扩散和流模型的扩展，采用学生`t`先验。这种方法与标准的高斯扩散模型兼容，并且只需要最小的代码更改。实证结果表明，`t-EDM`和`t-Flow`在高分辨率天气数据集上的重尾估计方面优于标准扩散模型，其中生成罕见和极端事件至关重要。

![](img/ee8d7e2637c4c8dbb2d3375583479b3f.png)

### A Comparative Study on Reasoning Patterns of OpenAI's o1 Model

贡献人：

原文链接：https://arxiv.org/abs/2410.13639

推文链接：[https://x.com/siweiwu7/status](https://x.com/siweiwu7/status/1848013413060051303)/1848013413060051303

研究节点：模型体系- 推理体系
