
# 奇绩大模型日报（12月 12日）

> 来源：[https://miracleplus.feishu.cn/docx/E5ocdZ72kosDTAx250GcFgShnab](https://miracleplus.feishu.cn/docx/E5ocdZ72kosDTAx250GcFgShnab)

## 资讯

### 谷歌发布新一代大模型

贡献人：

https://mp.weixin.qq.com/s/c39MbjULBW5M_8vdO-VUXQ

谷歌发布了新一代大模型Gemini 2.0，由CEO皮猜宣布，专为AI Agent设计，已提供给开发者内测，并将在Gemini和搜索产品线中集成。Gemini 2.0 Flash实验版模型网页端已开放，支持多模态输入输出，包括图像、视频、音频，以及可控的多语种文本到语音输出。它还能调用谷歌搜索、代码工具等。谷歌还推出了深度研究功能，基于高级推理和长上下文能力，能生成复杂主题的研究报告。Gemini 2.0在多模态图片、视频能力以及编码、数学等方面表现超越前代，速度是1.5 Pro的两倍。明年，Gemini 2.0将扩展到更多产品中，如Project Astra，提升对话、使用新工具、记忆力和改进延迟。Project Astra将移植到眼镜等移动终端，并与游戏开发商合作，测试基于Gemini 2.0的游戏智能体。Gemini 2.0 Flash实验模型通过Google AI Studio和Vertex AI的Gemini API向开发者提供，展示视频理解、工具使用、空间理解的应用。

![](img/e3c41b5f06c5eaa06497f901dbd4b331.png)

### RL+Control 如何将机器人可靠性逼进 99.9%？

贡献人：

https://mp.weixin.qq.com/s/DeB6t9304gxQmkAI82CrrA

本文是关于“RL+Control：将机器人可靠性逼近99.9%”的线上圆桌沙龙的总结。讨论的核心是如何利用强化学习（RL）和基于模型的控制（MBC）提高机器人的稳定性和可靠性。专家们认为，机器人的可靠性不仅涉及硬件稳定性，还包括恢复能力，且需结合场景需求。强化学习与控制的结合是提高稳定性的可行路径，如将控制的安全性融入RL或借助控制结构使RL更稳健。

在机器人移动（Locomotion）方面，仿真到现实强化学习（Sim2Real RL）是流行的方法，它利用仿真环境中训练的策略并部署到现实世界。Sim2Real RL的优势在于其离线计算能力和绕开状态估计问题，但与基于模型的控制（MBC）相比，它在算力运用和状态估计上有所不同。石冠亚提出，RL和Control应视为问题而非方法，强调了RL在真实世界学习的重要性，并分享了结合RL和Control的工作，如人形机器人控制和基于MPC的内容。

朱秋国讨论了从传统MPC、ZMP方法到现在使用RL的转变，认为RL能更好地展现机器人全身运动能力。他提到，尽管传统方法稳定可靠，但RL提供了更高的灵活性和协调性。卢宗青则从强化学习算法研究的角度出发，强调了与物理世界的交互和视觉信息的重要性，并提出了基于视觉信息的姿态生成模型。

罗剑岚强调了机器人操作（Manipulation）的挑战，指出其与Locomotion的不同，认为Manipulation需要应对外部世界的无限复杂性，而RL通过与外界交互能更好地适应这种不确定性。他提出，要实现高性能操作并达到高可靠性，最通用、最具扩展性的方法就是强化学习。

专家们还讨论了机器人可靠性的衡量问题，认为人形机器人要达到99%以上的可靠性极具挑战，且对于不同的应用场景，对可靠性的要求也不同。他们认为，机器人的可靠性需要结合硬件、软件算法以及场景需求来综合评估，并期待通过不断的技术迭代和创新来提高机器人的稳定性和可靠性。

![](img/50efeb915522ad199467762eacfb0fd7.png)

### Scaling Laws, O1 Pro Architecture, Reasoning, Training Infrastructure, Orion, and Claude 3.5 Opus Failures

贡献人：

https://semianalysis.com/2024/12/11/scaling-laws-o1-pro-architecture-reasoning-training-infrastructure-orion-and-claude-3-5-opus-failures/#scaling-inference-compute-through-search

文章深入探讨了多项技术进展及其挑战，特别是针对AI推理计算的优化和硬件架构方面的细节。文章首先分析了"Scaling Laws"的最新发展，指出随着模型规模的增加，推理计算需求呈指数级增长，这使得计算基础设施的需求也变得更加庞大。为了应对这种情况，提出了O1 Pro架构，这一架构通过提升计算单元的并行度和增强数据处理能力，在推理任务中取得了显著的性能提升。

在推理计算的过程中，文章详细讨论了模型推理的技术瓶颈。随着AI模型参数的不断增加，推理计算的效率成为影响模型性能的关键因素。推理计算的瓶颈主要集中在存储带宽和计算单元的并行能力上，而这些瓶颈的存在限制了模型规模的进一步扩大。为此，新的硬件架构尝试通过优化内存访问和计算调度来提升推理速度，从而满足大规模AI模型的计算需求。

同时，文章还探讨了"Reasoning"技术的最新进展。通过对推理算法和优化方法的调整，AI系统能够更好地理解和处理复杂的任务，尤其是在自然语言处理领域。在推理过程中，O1 Pro架构通过改进的流水线调度和算力优化，使得在更复杂的推理任务中，AI能够提供更准确和高效的回答。

文章还强调了训练基础设施的挑战，特别是在应对大规模AI模型时，训练所需的计算资源呈现出指数级增长，这导致传统计算架构已经无法满足需求。为了解决这个问题，O1 Pro架构不仅在推理阶段进行了优化，还在训练阶段实现了更高效的计算和资源调度。训练过程中的并行性和调度策略变得尤为重要，尤其是在大规模数据集和复杂模型的情况下，O1 Pro架构能够提供更强的计算能力和资源管理。

在分析了推理计算和训练基础设施的最新进展后，文章进一步讨论了Orion和Claude 3.5 Opus的技术问题。Orion在推理计算中遇到了性能瓶颈，尽管硬件架构进行了优化，但由于模型复杂性增加，仍未能达到预期的性能目标。而Claude 3.5 Opus在推理计算中的失败主要源于其架构未能充分利用新型硬件的优势，导致推理计算无法高效执行，进一步证明了硬件和软件优化需要更加紧密的配合。

![](img/9e330903b750561afab061914c782e2a.png)

### Generative Video WorldSim, Diffusion, Vision, Reinforcement Learning and Robotics — ICML 2024 Part 1

贡献人：

https://www.latent.space/p/icml-2024-video-robots

内容涵盖了2024年ICML大会的一些重要技术讨论，重点涉及生成性视频模型、扩散模型、计算机视觉、强化学习以及机器人技术等前沿话题。文章中首先介绍了OpenAI的Sora模型，该模型结合了Diffusion Transformers技术，展示了如何通过特定的归纳偏差在生成视频领域取得显著的进展。Bill Peebles在ICML上的报告详细讲解了Sora的架构和应用，并提出了该模型在引入这些偏差时所面临的挑战。

Google DeepMind在视频生成领域也表现突出，带来了两篇获奖论文：“Genie: Generative Interactive Environments”和“VideoPoet: A Large Language Model for Zero-Shot Video Generation”，展示了通过大型语言模型生成视频的零样本能力，这标志着视频生成技术迈向了新的高度。文章进一步探讨了扩散模型在生成任务中的应用，Sander Dieleman的报告强调了扩散模型在频域中的回归应用，这为图像和视频生成提供了新的思路。

另外，文章提到了一些流匹配（Flow Matching）技术的进展，特别是在高分辨率图像合成中的应用，Stable Diffusion 3便是其中的一个重要实例。Ben Poole介绍了如何通过2D先验推断3D结构，结合NeRFs和DreamFusion技术，推动了计算机视觉的边界。

在机器人领域，Chelsea Finn的四场报告则聚焦于如何利用视频数据训练机器人，并发展出了具有自主适应能力的机器人通用智能。她提出，机器人领域的进展可以为机器学习带来新的见解，特别是在如何将视频和视觉语言模型结合起来进行行为生成和任务执行方面。

![](img/d07d6bc0fb4d8822679e0aecd1018c5f.png)

## 推特

### ChatGPT 现已集成到 iOS、iPadOS 和 macOS 的 Apple 体验中

https://x.com/OpenAI/status/1866943282795938013

ChatGPT 现已集成到 iOS、iPadOS 和 macOS 的 Apple 体验中，用户可以直接在操作系统中使用 ChatGPT 的功能。

![](img/8618fa1e664e34910d3fb0e26fd55487.png)

[BQFVFYHf3FZ_K8Pd.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EPBXb6YkuovTAlx4e77cDnoYnxg)

### 使用 OpenAI Canvas 协作写作与编程

https://x.com/AndrewYNg/status/1866880693588070440

新短期课程：使用 OpenAI Canvas 协作写作与编程！

探索使用 OpenAI Canvas 的新方法，通过这一用户友好的界面，与 ChatGPT 协作进行头脑风暴、草稿撰写和文本与代码的精细化修改。

在这门短期课程中，由@OpenAI与 OpenAI 研究负责人@karinanguyen_共同开发并教授，你将学会如何利用 Canvas 提升工作流程效率。

Canvas 超越了简单的聊天交互，提供了一个并排的工作空间，便于你与 ChatGPT 协作编辑和优化文本或代码。这使得在写作或编码时的头脑风暴、草稿撰写与迭代更加自然高效。作为自 2022 年 ChatGPT 发布以来视觉界面的首次重大更新，Canvas 提供了与 AI 协作的全新创新方式。

例如，在编写代码的初版后，Canvas 可以审查并提供改进建议。它还可以通过添加日志记录、定位问题、编写注释等帮助调试。此外，你还将学习如何为 Canvas 这样的界面训练模型的相关知识。

在这个仅包含视频的短期课程中，你将：

• 学习如何请求逐行反馈，并通过直接编辑模型输出中的选定区域来控制工作的迭代。

• 学习如何使用快捷菜单中的快速自动化工具，调整文本语气和长度、优化代码，并恢复工作内容的历史版本。

• 学习如何将 Canvas 用作研究助手工具，例如请求模型推理关于截图图表的内容，并根据生成的报告继续提问。

• 学习如何让模型编写 Python 代码以复现截图中的图表。

• 了解如何从零开始创建一个视频游戏（如《太空战舰》），并对其进行编辑和以独立 HTML 文件形式展示。

• 获得创建 SQL 数据库（基于架构图片）的实际应用示例。

• 理解支持 Canvas 的模型训练和设计过程！

![](img/2847ca2471ab1345f661aaeb17d8857f.png)

[UghrQ7RQwULaMh0O.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/K4NjbClsDomabmx2ZnlcD1B9nAh)

### Midjourney推出Patchwork：实验性多人世界构建工具，用于搭建故事的基础框架

https://x.com/midjourney/status/1866964271948763553

今天，我们推出了一款全新的实验性多人世界构建工具——“Patchwork”。它结合了语言模型、图像模型以及基于画布的界面，用于搭建故事的基础框架。查看下面的 🧵 获取链接和更多文档信息。玩得开心！

![](img/957a2352988d19fed493ce2af7edf64c.png)

[XdD7DC7258D6grv7.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P5J0be5EEojPlzxIk4ncaaq3nOc)

### BLENDERGPT 全面上线，快捷生成 3D 资产

https://x.com/gd3kr/status/1866910744601825666

BLENDERGPT 现已全面上线，无需等待名单。

生成 3D 资产从未如此简单。以下是我在短短一分钟内完成整个 3D 场景的展示。

立即免费试用，访问：**blendergpt.org**

![](img/0a5201dfb989daf43f3fc1fb344f8505.png)

[1PPL36FMeOS7VkWh.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NHw9bXpUdofyLNxjd6tcdJF3nRd)

## 信号

### AMAGO-2: Breaking the Multi-Task Barrier in Meta-Reinforcement Learning with Transformers

贡献人：

原文链接：[h](https://x.com/__jakegrigsby__/status/1866987546355601875)ttps://x.com/__jakegrigsby__/status/1866987546355601875

学术报告链接：https://arxiv.org/abs/2411.11188

这篇文章探讨了“多任务障碍”，并说明诸如值分类等抗规模损失函数有助于修复它，而无需任何新的假设。强化学习可以通过借用序列建模的细节来改进，而不需要将强化学习变成监督学习。有一种RL 技巧可以将 Q 学习转化为分类，还可以快速解决多任务强化学习最不必要的问题：每个任务的训练损失的规模随着时间而变化不均匀。这种情况发生在监督学习中会很奇怪，而损失函数的改变可以在强化学习中修补它。多任务强化学习有更好的解决方案，但上下文适应需要修正，因为它最适合解决的问题涉及太多任务。我们会训练一个序列模型，以适应之前的尝试的记忆。这里的实验陷入了狭窄的领域。我们需要更多任务来泛化。

![](img/85e478ea9fc58abb1e421333516ccde2.png)

### Efficient Online Reinforcement Learning Fine-Tuning Need Not Retain Offline Data

贡献人：

学术报告链接：https://arxiv.org/abs/2412.07762v1

本篇论文研究没有离线数据保留的 RL 在线微调，称之为无保留微调的范式。论文对现有的离线到在线 RL 进行了详细分析方法并发现在微调过程中经常需要离线数据来减轻 Q 值发散以及由于分布偏移而导致的遗忘，而且也会逐渐减慢微调速度。论文演示一种结合预热阶段来初始化重放缓冲区的简单方法从预训练的离线强化学习策略进行少量转换，然后运行简单的在线强化学习该算法可以有效地进行样本高效的微调，而不会忘记预先训练的初始化。

![](img/4839a835ef4c9287c56df70b9e33acf5.png)

### Mobile-TeleVision: Predictive Motion Priors for Humanoid Whole-Body Control

贡献人：

学术报告链接：https://arxiv.org/abs/2412.07773v1

论文引入了 PMP（预测运动先验），使用条件变分自动编码器 (CVAE) 进行训练，以有效地表示上半身运动。运动策略是根据这种上半身运动表示进行训练的，确保系统在操作和运动方面都保持稳健。CVAE 特征对于稳定性和稳健性至关重要，并且在精确操作方面明显优于基于 RL 的全身控制。通过精确的上半身运动和强大的下半身运动控制，操作员可以远程控制人形机器人四处走动并探索不同的环境，同时执行各种操作任务。

![](img/db6cd6555e51048550e3da243ebe4475.png)

### FiVA: Fine-grained Visual Attribute Dataset for Text-to-Image Diffusion Models

贡献人：

学术报告链接：https://arxiv.org/abs/2412.07674v1

论文提出了一个全面的细粒度视觉属性数据集，其特征精心描绘各种视觉属性的图像。在注释现实世界图像时，论文建议利用先进的 2D 生成模型用于数据收集的自动数据生成管道。论文开发了一种系统方法，包括属性和主题定义、提示创建、基于 LLM 的过滤和人工验证构建一个数据集，在大约 1 M 幅图像中标注有不同的视觉属性。每张图片可以进一步分为由不同种子生成的 2x2 子图像，可用于采样期间训练。在此数据集的基础上，论文引入了细粒度的视觉属性适应框架旨在控制生成过程中的细粒度视觉属性。具体来说，论文建议将多模态编码器 Q-former 集成到图像特征编码器中在插入交叉注意模块之前。这种集成有助于理解标签或提取图像信息的简要说明。

![](img/16672d87549102d0a3218acc2ace3553.png)

### RLDG: Robotic Generalist Policy Distillation via Reinforcement Learning

贡献人：

学术报告链接：https://generalist-distillation.github.io/
