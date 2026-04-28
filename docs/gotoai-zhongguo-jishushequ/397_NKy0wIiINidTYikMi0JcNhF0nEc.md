# 李飞飞 2024｜AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

> 来源：[https://gotoaiworld.feishu.cn/docx/Ab5ndXO7QoR8ANxaTRIc4N5XnGc](https://gotoaiworld.feishu.cn/docx/Ab5ndXO7QoR8ANxaTRIc4N5XnGc)

TITLE：AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

LINK：https://arxiv.org/pdf/2401.03568

Li Fei-Fei

## 系统概览全图

![](img/3ffa8d6f2c5eb9bd67f26b358b511d81.png)

图1：在各个领域和应用场景中落地的Agent AI概览全图

> 图 1 为在各个领域和应用场景中落地的Agent AI[概览全图。A](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E9%80%9A%E7%94%A8%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLpgJrnlKjkurrlt6Xmmbrog70iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.tlV8C4ZSqRwIk52Y2bnlGswOeB4WNiosdAGPVtINB2Y&zhida_source=entity)g[ent A](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E5%A4%9A%E6%A8%A1%E6%80%81%E7%90%86%E8%A7%A3&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLlpJrmqKHmgIHnkIbop6MiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.ELpcnGs7KY3rsN26vG0kfZlWU2tJn9L22ECzFvxhFeQ&zhida_source=entity)I[正逐渐成为](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E8%B7%A8%E7%8E%B0%E5%AE%9E%E6%95%B0%E6%8D%AE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLot6jnjrDlrp7mlbDmja4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9._n559rGrrtUqP_AxDFE231I8gTQoqsi94DDVcgaba0c&zhida_source=entity)实现通用人工智能（Artificial General Intelligence, AGI） 的一条极具前景的路径。Agent AI训练已展现出实现物理世界中多模态理解的能力，它通过将生成式AI与多个独立数据源相结合，为与现实无关的训练提供了一个框架。针对agent及行动相关任务训练的大型基础模型，若在跨现实数据（cross-reality data）上进行训练，则可应用于物理世界与虚拟世界。本文呈现了一个可在众多不同领域及应用中感知与落地的Agent AI系统的总体概述，该系统或许能作为一种借助智能体范式（agent paradigm）迈向AGI的路径。

---

## 5 Agent AI分类

### 5.1 通用智能体领域（Generalist Agent， GAs）

基于计算机的动作与通用智能体可应用于多种任务。大型基础模型与交互式AI领域的近期进展，为通用智能体赋予了新功能。然而，通用智能体要真正对用户产生价值，必须具备自然的交互性，并能在广泛的场景与模态中实现泛化。我们在第6节中设置关于Agent基础AI的高质量扩展主章节，聚焦与这些主题总体相关的领域：

多模态Agent AI（Multimodal Agent AI, MMA） 是一个新兴论坛，旨在促进研究界与工业界之间，以及与更广泛的Agent AI研究和技术社区之间的交流合作。大型基础模型与交互式AI领域的近期进展，为通用智能体（GAs）带来了新功能，例如在受限场景中预测用户动作与任务规划（如MindAgent（Gong et al., 2023a）、细粒度多模态视频理解（Luo et al., 2022）、机器人技术（Ahn et al., 2022b; Brohan et al., 2023）），或为用户提供整合知识反馈的聊天伙伴（如医疗系统的网站客户支持（Peng et al., 2023））。以下将详细介绍代表性研究及最新代表性成果。我们希望借此探讨对多模态Agent AI（MAA）未来的愿景，并激励未来研究者投身该领域。本文及我们的论坛涵盖以下主要主题，但不仅限于此：

+   核心主题：多模态Agent AI、通用Agent AI

+   [次要主题：](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E5%8A%A8%E4%BD%9C%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLliqjkvZzmmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.NbVWswGgTWrXWG0gkZ0upcLZBEpbPRSIjSuB4w7_O-4&zhida_source=entity)具身智能体（Embodied Agents）、动作智能体（Action Agents）、基于语言的智能体（Language-based Agents）、视觉与语言智能体（Vision & Language Agents）、知识与推理智能体（Knowledge and Inference Agents）、面向游戏、机器人技术、医疗健康等领域的智能体（Agents for Gaming, Robotics, Healthcare, etc.）

+   扩展主题：视觉导航（Visual Navigation）、仿真环境（Simulation Environmen[ts）、](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLomZrmi5_njrDlrp4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Lnz7--QdasjhZNrEUthfqYRx4y8uHY_nteHisLg_XCo&zhida_source=entity)重[排任务（](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E5%A2%9E%E5%BC%BA%E7%8E%B0%E5%AE%9E&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLlop7lvLrnjrDlrp4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.X-77uIdiJGRPviJ-M8oPF9IEJUDqGZP-txNxeV81cPI&zhida_source=entity)R[earr](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E6%B7%B7%E5%90%88%E7%8E%B0%E5%AE%9E&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLmt7flkIjnjrDlrp4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.LKgF0eyyARfZ_RI3yAlUfpMdmyLY6J3McKBz5AthAAY&zhida_source=entity)angement）、智能体化基础模型（Agentic Foundation Models）、虚拟现实/增强现实/混合现实（VR/AR/MR）、具身视觉与语言（Embodied Vision & Language）

接下来，我们列出具体的代表性智能体分类如下：

### 5.2 具身智能体（Embodied Agents）

人类的生物大脑存在于身体之中，而身体在不断变化的世界中活动。具身人工智能（embodied artificial intelligence） 的目标是创建如机器人这类智能体，使其能够通过学习创造性地解决需要与环境交互的复杂任务。尽管这是一项重大挑战，但深度学习的重要进展以及ImageNet等大型数据集的日益普及，已使AI在多个此前被认为难以解决的任务上实现了超人类性能（superhuman performance）。计算机视觉、语音识别和自然语言处理在语言翻译、图像分类等被动输入输出任务中经历了变革性突破，强化学习也在游戏等交互式任务中取得了世界级表现。这些进展为具身AI注入了强大动力，使越来越多的研究者能够在开发可与机器交互的智能体方面取得快速进展。

5.2.1 动作智能体（Action Agents）

动作智能体指需要在模拟物理环境或现实世界中执行物理动作的智能体，其核心特征是需主动与环境进行活动交互。根据应用领域，我们将动作智能体大致分为两类：游戏AI和机器人技术。

在游戏AI中，智能体将与游戏环境及其他独立实体交互。在这类场景中，自然语言可实现智能体与人类之间的顺畅沟通。根据游戏类型不同，可能需要完成特定任务以获得真实奖励信号。例如，在竞技类游戏《外交》（Diplomacy）中，使用人类对话数据训练语言模型，并结合基于强化学习的动作策略，可使智能体达到人类水平的游戏表现 (Meta Fundamental AI Research (FAIR) Diplomacy Team et al., 2022)。

此外，还存在智能体作为城镇普通居民活动的场景（Park et al., 2023a），这类场景中智能体无需优化特定目标。基础模型在这类场景中发挥着重要作用，因为它们可通过模仿人类行为，模拟出更自然的交互过程。当配备外部记忆（external memory）时，这些模型能生成具有说服力的智能体——它们可进行对话、制定日程、建立关系并拥有虚拟生活。

5.2.2 [交互式智能体](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E4%BA%A4%E4%BA%92%E5%BC%8F%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLkuqTkupLlvI_mmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.sF6Lg0nMuo75YcRTon0dGsRh6TsR286O7Kd3fAfzd9w&zhida_source=entity)（Interactive Agents）

交互式智能体泛指可与世界交互的智能体，其范围比动作智能体更广。它们的交互形式不一定需要物理动作，可能包括向用户传递信息或改变环境。例如，一个具身交互式智能体可通过对话回答用户关于某主题的问题，或像聊天机器人（chatbot）一样帮助用户解析现有信息。通过将信息共享纳入智能体的能力范围，Agent AI的核心设计与算法可有效适配多种应用，例如诊断智能体（Lee et al., 2023）和知识检索智能体（Peng et al., 2023）。

### 5.3 [仿真与环境智能体](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E4%BB%BF%E7%9C%9F%E4%B8%8E%E7%8E%AF%E5%A2%83%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLku7_nnJ_kuI7njq_looPmmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.YMuBfW6QSGAhfJtmAkIXV5uRbytAE_h39k1oLHmDCKA&zhida_source=entity)（Simulation and Environments Agents）

AI智能体学习在环境中如何行动的一种有效方法，是通过与环境交互获取试错经验（trial-and-error experiences）。强化学习（RL） 是代表性方法，其训练智能体需要大量失败经验。尽管存在使用物理智能体（physical agents）的方法（Kalashnikov et al., 2018），但物理智能体的使用既耗时又昂贵。此外，当在实际环境中失败可能存在危险时（如自动驾驶、水下航行器），在物理环境中训练往往不可行。因此，使用模拟器（simulators） 学习策略是一种常用方法。

已有许多为具身AI研究设计的仿真平台，涵盖从导航（Tsoi et al., 2022; Deitke et al., 2020; Kolve et al., 2017）到物体操作（Wang et al., 2023d; Mees et al., 2022; Yang et al., 2023a; Ehsani et al., 2021）等领域。例如，Habitat（Savva et al., 2019; Szot et al., 2021）提供了3D室内环境，人类智能体和机器人智能体可在其中执行导航、指令跟随、问答等多种任务。另一个代表性仿真平台是VirtualHome（Puig et al., 2018），支持人类虚拟化身（human avatars）在3D室内环境中进行物体操作。在游戏领域，Carroll等人推出了“Overcooked-AI”——一个用于研究人机协作任务的基准环境（Carroll et al., 2019）。与此类似，多项研究致力于将真实人类干预融入其中，超越智能体与环境交互的单一焦点（Puig et al., 2023; Li et al., 2021a; Srivastava et al., 2022）。这些模拟器有助于在涉及智能体与机器人交互的实际场景中学习策略，也支持利用人类演示动作的基于模仿学习的策略学习（IL-based policy learning）。

在某些场景中，学习策略的过程可能需要在模拟器中整合专用特征（specialized features）。例如，在学习基于图像的策略时，通常需要真实感渲染（realistic rendering） 以提升对真实环境的适应性（Mittal et al., 2023; Zhong et al., 2023）。使用真实感渲染引擎（realistic rendering engine）能有效生成反映光照环境等多种条件的图像。此外，还需要采用物理引擎（physics engines） 的模拟器来仿真与物体的物理交互（Liu and Negrut, 2021）。研究表明，在仿真中整合物理引擎有助于习得可应用于现实场景的技能（Saito et al., 2023）。

### 5.4 [生成式智能体](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E7%94%9F%E6%88%90%E5%BC%8F%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLnlJ_miJDlvI_mmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Q9fNfFfIIivynVijdI1JEX5-l7lZA0jR05CntzjYD8c&zhida_source=entity)（Generative Agents）

大型生成式AI模型领域的最新进展，有望大幅降低当前交互式内容的高成本与耗时问题——这不仅利好大型游戏工作室，也能赋能小型独立工作室，让他们创建出超出当前能力范围的高质量体验。此外，将大型AI模型嵌入沙盒环境，能让用户自主创作体验，以目前无法实现的方式发挥创造力。

这类智能体的目标不仅是为场景添加交互式3D内容，还包括：

+   为物体添加任意行为和交互规则，使用户只需少量提示即可创建自定义虚拟现实（VR）规则；

+   通过多模态GPT4-v模型及其他包含视觉AI模型的模型链，根据纸上草图生成完整的关卡几何结构；

+   利用扩散模型（diffusion models）为场景内容重新纹理化；

+   根据用户的简单提示创建自定义着色器和视觉特效。

短期内的一个潜在应用是，开发VR故事板/原型工具，让单个用户能以比当前快一个数量级的速度创建体验/游戏的粗略（但可用）草图。利用这些工具，此类原型还可进一步扩展和优化，变得更加完善。

5.4.1 增强现实/虚拟现实/混合现实智能体（AR/VR/mixed-reality Agents）

增强现实/虚拟现实/混合现实（统称XR）场景目前需要专业艺术家和动画师创建角色、环境和物体，以模拟虚拟世界中的交互。这是一个成本高昂的过程，涉及概念艺术、3D建模、纹理绘制、绑定和动画制作。XR智能体（XR agents） 可通过促进创作者之间的交互，以及构建工具辅助创建最终虚拟环境，来助力这一过程。

我们的早期实验已证明，GPT模型可在Unity引擎中以少样本模式（无需额外微调）调用引擎特定方法、通过API调用从互联网下载3D模型并将其放置到场景中，以及为这些模型分配行为和动画状态树（Huang et al., 2023a）。这种能力的出现，可能是因为开源游戏仓库中存在使用Unity的类似代码。因此，GPT模型能够根据用户的简单提示，通过在场景中加载大量物体，构建丰富的视觉场景。

这类智能体的目标是构建一个平台和一套工具，在大型AI模型（包括GPT系列模型和扩散图像模型）与渲染引擎之间提供高效接口。我们主要探索两个方向：

+   将大型模型整合到智能体架构的各类编辑器工具中，显著提升开发速度；

+   在用户体验中控制渲染引擎——通过生成符合用户指令的代码并在运行时编译，使用户能够以任意方式编辑正在交互的VR/仿真环境，甚至可引入新的智能体机制。

为XR场景引入专注于该领域的AI copilot，对XR创作者而言极具价值。创作者可利用AI copilot完成繁琐任务（如提供简单资产或编写代码模板），从而专注于创意构想并快速迭代想法。

此外，智能体还能帮助用户通过添加新资产、改变环境动态或构建新场景，以交互方式修改环境。这种运行时的动态生成也可由创作者指定，让用户的体验保持新鲜感并随时间不断演进。

### 5.5 [知识与逻辑推理智能体](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E4%B8%8E%E9%80%BB%E8%BE%91%E6%8E%A8%E7%90%86%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLnn6Xor4bkuI7pgLvovpHmjqjnkIbmmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.tGerCqWdAH8h83woC6fj8INUh-f18yI64uruQRg9hGE&zhida_source=entity)（Knowledge and Logical Inference Agents）

推论与应用知识的能力是人类认知的标志性特征，在逻辑演绎（logical deduction）、心智理论（theory of mind）理解等复杂任务中尤为明显。对知识进行推理，可确保AI的响应和动作与已知事实及逻辑原则保持一致。这种连贯性是维持AI系统可信度和可靠性的关键机制，在医疗诊断（medical diagnosis）、法律分析（legal analysis）等关键应用中尤为重要。本节将介绍整合知识与推理交互的智能体，这类智能体旨在解决智能与推理的特定方面问题。

5.5.1 知识智能体（Knowledge Agent）

知识智[能体（Kno](https://zhida.zhihu.com/search?content_id=264709285&content_type=Article&match_order=1&q=%E5%A4%A7%E5%9E%8B%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjI3NjA1NzEsInEiOiLlpKflnovor63oqIDmqKHlnosiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjQ3MDkyODUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.5TXJU07epM-GwmtMD3z_A244Xh4DRZVWElcibilIGOU&zhida_source=entity)wledge Agents）对其获取的知识系统进行双向推理：隐性推理（implicit） 与显性推理（explicit）。隐性知识（implicit knowledge） 通常是GPT系列（Brown et al., 2020; OpenAI, 2023）等大型语言模型在海量文本数据上训练后所蕴含的知识。这些模型能生成看似理解内容的响应，因为它们会利用训练过程中隐性学习到的模式和信息。相反，显性知识（explicit knowledge） 具有结构化特征，可直接查询，例如知识库或数据库中的信息——传统上，这类知识通过引用可验证的外部资源来增强AI的推理能力。

尽管语言模型取得了进展，但其隐性知识是静态的，会随着世界的发展而过时（Lewis et al., 2020; Peng et al., 2023）。这一局限性使得整合持续更新的显性知识来源成为必要，以确保AI系统能提供准确且及时的响应。隐性知识与显性知识的融合，让AI智能体具备更细致的理解能力，以及结合上下文应用知识的能力，类似人类智能（Gao et al., 2022）。这种整合对于构建以知识为中心的AI智能体至关重要——这类智能体不仅拥有信息，还能理解、解释并运用信息，从而缩小广泛学习与深度知识之间的差距（Marcus and Davis, 2019; Gao et al., 2020）。这些智能体旨在结合世界的灵活性和动态信息进行推理，以提升自身的鲁棒性和适应性（Marcus, 2020）。

5.5.2 逻辑智能体（Logic Agents）

通常而言，逻辑智能体（logic agent） 是系统的一个组件，旨在应用逻辑推理（logical reasoning） 处理数据，或解决特定于逻辑推理（logical inference）的任务。在GPT-4等大型基础模型的语境下，逻辑智能体指专门用于处理逻辑推理任务的特定组件或子模块。这些任务通常涉及理解和处理抽象概念、从给定前提推导结论，或解决需要结构化逻辑方法的问题。从广义上看，GPT-4等基础模型在海量文本语料上训练，能够执行多种任务，包括那些需要某种形式逻辑推理的任务。因此，它们的逻辑推理能力被整合到整体架构中，通常并不具备一个独立、孤立的“逻辑智能体”。

尽管GPT-4及类似模型能执行涉及逻辑的任务，但其运作方式与人类或传统基于逻辑的系统存在根本差异。它们不遵循形式逻辑规则（formal logical rules），也不具备对逻辑的显性理解；相反，它们基于从训练数据中学习到的模式生成响应。因此，它们在逻辑任务中的表现可能令人印象深刻，但也可能因训练数据的性质和模型设计的固有局限性而表现出不一致性或局限性。在架构中嵌入独立逻辑子模块的一个例子是（Wang et al., 2023e）——该研究通过将文本解析为逻辑片段（logical segments），并在token embeddings中显性建模逻辑层次结构（logical hierarchies），修改了LLM在预训练阶段使用的token embeddings过程。

5.5.3 情感推理智能体（Agents for Emotional Reasoning）

在许多人机交互场景中，情感理解（emotional understanding）和共情（empathy）是智能体的重要技能。例如，创建具有吸引力的对话智能体（dialogue agents）的一个重要目标，是让智能体在表现出更强情感和共情的同时，最大限度减少社交不当或冒犯性输出。为推进对话智能体的这一目标，我们发布了神经图像共情评论（Neural Image Commenting with Empathy, NICE）数据集（Chen et al., 2021），该数据集包含近200万张图像、对应的人类生成评论以及一组人类情感标注。我们还提出了一种新的预训练模型——图像评论情感生成建模（Modeling Affect Generation for Image Comments, MAGIC）（Chen et al., 2021），其目标是为图像生成评论：该模型以捕捉风格和情感的语言表征为条件，助力生成更具共情性、情感性、吸引力和社交恰当性的评论。我们的实验表明，该方法能有效训练出更类人、更具吸引力的图像评论智能体。

开发具有共情感知能力的智能体（empathy-aware agents）是交互式智能体的一个有前景方向，且构建能对广泛群体进行情感理解的智能体至关重要——尤其需要注意的是，当前许多语言模型在情感理解和共情推理能力上存在偏差（Mao et al., 2022; Wake et al., 2023d）。

5.5.4 神经符号智能体（Neuro-Symbolic Agents）

神经符号智能体（Neuro-Symbolic agents） 基于神经元与符号的混合系统（hybrid system of neurons and symbols）运行（d’Avila Garcez and Lamb, 2020）。解决以自然语言表述的问题是一项挑战，因为这需要显式捕捉输入中隐含的离散符号结构信息。然而，大多数通用神经序列模型不会显式捕捉这类结构信息，这限制了它们在这些任务上的性能。研究（Chen et al., 2020）提出了一种基于结构化神经表征智能体（structured neural representation agent）的新型编码器-解码器模型（encoder-decoder model）：TP-N2F的编码器采用TPR“绑定”（TPR ‘binding’）在向量空间中编码自然语言符号结构，解码器则使用TPR“解绑”（TPR ‘unbinding’）在符号空间中生成由关系元组表示的序列程序，每个元组包含一个关系（或操作）和若干参数。

像GPT-4这样遵循指令的视觉-语言（VL）模型提供了灵活的接口，能以零样本方式支持广泛的多模态任务。然而，基于完整图像运行的接口无法直接让用户“指向”并访问图像中的特定区域。这种能力不仅对支持基于指代的视觉-语言基准（reference-grounded VL benchmarks）很重要，对需要精确图像内推理的实际应用也至关重要。

在（Park et al., 2023b）中，我们构建了局部化视觉常识模型（Localized Visual Commonsense model），允许用户指定（多个）区域作为输入。我们通过从大型语言模型（LLM）中采样局部化常识知识（localized commonsense knowledge）来训练模型：具体而言，我们提示LLM根据由一组VL模型自动生成的全局文本图像描述和局部文本区域描述，收集常识知识。该流程具有可扩展性且完全自动化，因为不需要对齐的或人工编写的图像-文本对。借助单独训练的用于选择高质量示例的评判模型，我们发现仅在从图像扩展而来的局部化常识语料库上训练，就能成功蒸馏现有VL模型，使其支持以指代为输入的接口。零样本设置下的实证结果和人类评估表明，与传递生成的指代表达式（referring expression）的基线相比，我们的蒸馏方法能得到推理更精确的VL模型。

### 5.6 大型语言模型与视觉语言模型智能体（LLMs and VLMs Agent）

多项研究利用大型语言模型（LLMs）作为智能体执行任务规划（Huang et al., 2022a; Wang et al., 2023b; Yao et al., 2023a; Li et al., 2023a），并借助LLMs的互联网级领域知识和零样本规划能力执行智能体任务，如规划和推理。近期机器人研究也利用LLMs进行任务规划（Ahn et al., 2022a; Huang et al., 2022b; Liang et al., 2022），将自然语言指令分解为一系列子任务（形式可为自然语言或Python代码），然后使用低层控制器执行这些子任务。此外，（Huang et al., 2022b）、（Liang et al., 2022）和（Wang et al., 2023a）还整合了环境反馈以提升任务性能。另有多项研究表明，在大规模文本、图像和视频数据上训练的通用视觉对齐大型语言模型（visually-aligned large language models），可作为构建具身且能在多种环境中行动的多模态智能体的基础（Baker et al., 2022; Driess et al., 2023; Brohan et al., 2023）。

## 6 Agent AI应用任务（Agent AI Application Tasks）

### 6.1 面向游戏的智能体（Agents for Gaming）

游戏为测试大型语言模型（LLMs） 和视觉语言模型（VLMs） 的智能体行为提供了独特的沙盒环境，以扩展其协作能力和决策能力的边界。我们重点描述三个领域，以凸显智能体与人类玩家及其他智能体的交互能力，以及它们在环境中采取有意义动作的能力。

6.1.1 非玩家角色（NPC）行为

在现代游戏系统中，非玩家角色（Non-Player Characters, NPCs） 的行为主要由开发者编写的预定义脚本决定。这些脚本包含游戏环境中基于各种触发条件或玩家动作的一系列反应与交互。然而，这种脚本化特性往往导致NPC行为可预测或重复，无法根据玩家动作或游戏动态环境进行演化。这种刚性限制了动态游戏环境中预期的沉浸体验。因此，对利用LLMs提升NPC行为的自主性（autonomy） 和适应性（adaptability） 的兴趣日益浓厚，旨在使交互更细致、更具吸引力。

AI驱动的NPC可从玩家行为中学习，适应不同策略，提供更具挑战性且可预测性更低的游戏体验。大型语言模型（LLMs）能显著推动游戏中NPC行为的演化：通过处理海量文本，LLMs可学习模式并生成更多样、更类人的响应；它们可用于构建动态对话系统（dynamic dialogue systems），使与NPC的交互更具吸引力且更难预测；此外，LLMs可通过玩家反馈和游戏内数据进行训练，持续优化NPC行为，使其更符合玩家期望和游戏动态。

![](img/afe20ff3770d0c4e5d84448eeebbd8cd.png)

图8：面向用户交互式游戏动作预测与交互编辑的具身智能体。该智能体通过GPT-4V实现《我的世界：地下城》（Minecraft Dungeons）游戏感知仿真与生成。

6.1.2 人类-非玩家角色（NPC）交互（Human-NPC Interaction）

人类玩家与NPC的交互是游戏体验的关键方面。传统交互范式主要是一维的（one-dimensional），NPC仅以预设方式对玩家输入做出反应。这种限制扼杀了更自然、更丰富交互的潜力——这类交互本可类似虚拟领域中的人际交互。LLM和VLM技术的出现有望改变这一范式：通过应用这些技术，游戏系统可分析并从人类行为中学习，提供更类人的交互。这不仅提升了游戏的真实感和吸引力，还为在可控且复杂的场景中探索和理解人机交互提供了平台。

6.1.3 基于智能体的游戏分析（Agent-based Analysis of Gaming）

游戏是日常生活的重要组成部分，据估计全球有一半人口参与游戏活动。此外，游戏对心理健康也有积极影响。然而，当代游戏系统在与人类玩家的交互方面存在不足，因为其行为主要由游戏开发者手工设计。这些预编程行为往往无法适应玩家需求。因此，游戏领域需要新的AI系统，能够分析玩家行为并在必要时提供适当支持。

智能交互系统（intelligent interactive systems）有望从整体上革新玩家与游戏系统的交互方式：NPC与玩家的交互不再受限于游戏开发者设计的有限规则集，它们有潜力无缝适配玩家体验，提供及时反馈以丰富游戏体验，并提升人机交互的协同性（synergy of human-machine interaction）。

LLMs可作为分析游戏内文本数据（包括聊天记录、玩家反馈和叙事内容）的强大工具，帮助识别玩家行为、偏好和交互模式——这些信息对游戏开发者改进游戏机制和叙事至关重要。此外，VLMs可解析游戏会话中的大量图像和视频数据，助力分析玩家在游戏世界中的意图和动作。不仅如此，LLMs和VLMs还能推动游戏内智能体的开发，使其能以复杂且类人的方式与玩家及其他智能体沟通，提升整体游戏体验。

除LLMs和VLMs外，用户输入数据为创建游戏智能体提供了有前景的方向——这类智能体通过模仿人类玩家，对感知、游戏玩法和游戏理解进行建模。通过整合玩家交互与反馈、像素输入以及自然语言规划与理解，智能体模型可助力游戏动态的持续改进，推动游戏环境向更以玩家为中心（player-centric）的方向演化。

![](img/cb7e4293db26c6fb2661a431c866ff57.png)

图9：GPT-4V在提示中给定动作历史和游戏目标时，能有效预测高层级下一步动作。此外，GPT-4V能准确识别出玩家持有木原木，并将感知信息整合到未来动作规划中。尽管能够预测一些低层级动作（如按E键打开背包），但并不适合原始低层级动作预测（包括鼠标移动），可能需要其他模块用于低层级动作控制。

6.1.4 游戏场景合成（Scene Synthesis for Gaming）

场景合成是创建和增强沉浸式游戏环境的重要组成部分。它涉及游戏中三维场景和环境的自动或半自动生成。这一过程包括地形生成、物体放置、真实感光照创建，有时甚至还包括动态天气系统。

现代游戏通常具有广阔的开放世界环境。手动设计这些景观可能极其耗时且耗费资源。自动化地形生成（常利用程序化或 AI 驱动技术）能以更少的人工投入生成复杂、逼真的景观。大语言模型（LLMs）和视觉语言模型（VLMs）可利用互联网级别的知识，来制定规则，设计出视觉效果突出且独一无二的非重复景观。此外，LLMs 和 VLMs 可用于确保生成资产的语义一致性和多样性。以真实且美观的方式在场景中放置建筑物、植被等元素，对沉浸感至关重要。VLMs 和 LLMs 可通过遵循预设或习得的规则与美学标准辅助物体放置，从而加快关卡设计流程。VLMs 和 LLMs 还可进一步训练以理解设计原则和美学，助力内容的程序化生成。它们能帮助制定规则或指南，供程序化算法遵循，以生成既美观又符合场景语境的物体和场景。

![](img/2863cebdefcb54b9c0622e89218a4944.png)

图 10：对未见过的《我的世界》（Minecraft）视频进行掩码视频预测（masked video prediction）。从左到右：原始帧、掩码帧、重建帧、带补丁的重建帧。

真实感光照和大气效果是创建可信且吸引人的游戏环境的基础。先进算法可模拟自然光照条件和动态天气效果，增强场景的真实感和氛围。LLMs 能通过多种创新方式帮助开发系统，以实现更真实的光照和大气效果。VLMs 可分析来自现实世界光照和大气条件的海量数据集，帮助开发更真实的算法，用于在游戏中模拟这些效果。通过理解自然光照和天气的模式与细节，这些模型能助力开发出高度模拟现实的算法。LLMs 和 VLMs 还可用于开发系统，根据玩家动作、游戏状态或外部输入实时调整光照和大气效果。它们能处理玩家的自然语言指令来修改游戏环境，提供更具交互性和沉浸感的体验。

6.1.5 实验与结果

基于 LLM 或 VLM 的零样本 / 少样本学习。正如我们在图 8 和图 9 中所示，我们使用 GPT-4V 进行高级描述和动作预测。图 8 展示了一些使用 GPT-4V 生成和编辑动作描述的定性示例。智能体增强文本为生成带有游戏动作先验的 3D 场景开辟了新方法，有助于提升场景的自然度。因此，GPT-4V 生成的相关高级描述与游戏视频相匹配。

小型智能体预训练模型（Small Agent Pretraining Model）。为了展示我们的智能体视觉 - 语言架构，我们首先研究了其在一个广泛使用的游戏智能体领域中的应用 —— 通过在《我的世界》（Minecraft）数据上进行预训练。如图 7 所示，给定输入动作智能体、视频关键帧及对应文本，可采用标准的编码器 - 解码器将智能体动作和图像转换为动作文本令牌（token）和图像补丁令牌，然后使用智能体-视觉-语言解码器将其转换为动作预测语句。整体架构如图 7 所示。我们通过多个《我的世界》演示来评估该方法。《我的世界》视频数据包含 5 分钟的片段，用于预训练的有 7.8 万个视频，我们使用 5000 个视频（占预训练数据的 6%）进行首轮预训练。我们在 16 块 NVIDIA v100 GPU 上训练了一个 2.5 亿参数的模型，训练时长为一天，并在图 10 和图 11 中可视化了模型输出。图 10 显示，我们相对小型的智能体架构能够对训练期间未见过的《我的世界》场景生成合理输出。图 11 展示了模型预测结果与真实人类玩家动作的对比，表明我们的小型智能体模型具备潜在的低层次理解能力。

![](img/5ecfc6f67e0d532130a3227d4a295b36.png)

图 11：在《我的世界》游戏场景中，小型智能体预训练模型的低层次下一步动作预测。

多智能体架构（Multi-Agent Infrastructure）。正如在图 5 的智能体范式中所示，我们为一个名为 “CuisineWorld”（Gong 等人，2023a）的新游戏场景设计了一种新型架构。我们在图 12 中详细说明了该方法。我们的架构通过利用 GPT-4 作为中央规划器，支持多智能体协作，并能在多个游戏领域中运行。我们研究了系统的多智能体规划能力，并将该架构部署到真实视频游戏中，以展示其在多智能体协作及人机协作方面的有效性。此外，我们提出了 “Cuisineworld”—— 一个基于文本的多智能体协作基准，它提供了一种新的自动度量指标 “协作分数（CoS，Collaboration Score）” 来量化协作效率。

更多关于游戏描述、高级动作预测和 GPT-4V 提示词的示例及细节，请参见附录。我们在图 32 及附录 B 中展示了《Bleeding Edge》的示例，在图 33 及附录 C 中展示了《微软模拟飞行》（Microsoft Flight Simulator）的示例，在图 34 及附录 D 中展示了《刺客信条：奥德赛》（ASSASSIN’s CREED ODYSSEY）的示例，在图 35 及附录 E 中展示了《战争机器 4》（GEARS of WAR 4）的示例，在图 36 及附录 F 中展示了《星空》（Starfield）的示例。我们还在附录 A 的图 31 中提供了用于生成《我的世界》示例的 GPT-4V 提示词过程的详细截图。

### 6.2 机器人技术（Robotics）

机器人是需要与环境进行有效交互的典型智能体。本节将介绍机器人高效运行的关键要素，综述最新LLM/VLM技术的应用研究方向，并分享我们最新的研究成果。

![](img/159bec9a0af59911cbba110646c3e727.png)

图12：情境学习游戏架构中的MindAgent。规划技能与工具使用：生成相关游戏信息，并将游戏数据转换为LLMs可处理的结构化文本格式。LLM：架构核心组件，负责决策和多智能体系统调度器。记忆历史：用于存储相关信息。动作模块：从文本输入中提取动作，转换为领域特定语言（DSLs），验证以确保执行无错误。

视觉-运动控制（Visual Motor Control）视觉-运动控制指在机器人系统中整合视觉感知与运动动作，以高效执行任务。这种整合至关重要，它使机器人能够解析环境中的视觉数据，并据此调整运动动作，从而精准与环境交互。例如，在装配线上，配备视觉-运动控制的机器人能够感知物体的位置和姿态，并精确调整其机械臂以与这些物体交互。这种能力对于确保机器人在各类应用中的操作精度和效率不可或缺，涵盖从工业自动化到协助老年人日常家务等场景。此外，视觉-运动控制还能帮助机器人适应动态环境——在这类环境中，环境状态可能快速变化，需要根据视觉反馈实时调整运动动作。

同时，在安全操作语境下，视觉信息对检测执行错误、确认机器人每个动作的前置与后置条件至关重要。在非受控环境（如未知家庭场景）中，由于家具形状变化、光照差异、物体滑动等不可预测因素，机器人更可能面临意外结果。仅以前馈方式执行预规划动作计划，在这类场景中会带来显著风险。因此，利用视觉反馈在每一步持续验证结果，是确保机器人系统稳定可靠运行的关键。

语言条件控制操作（Language Conditioned Manipulation）语言条件控制操作指机器人系统基于语言指令解析并执行任务的能力。这一特性对构建直观、易用的人机交互界面尤为关键。通过自然语言指令，用户可以像人与人沟通一样向机器人下达目标和任务，从而降低机器人系统的操作门槛。在实际场景中，例如用户可指令服务机器人“从桌子上拿起那个红苹果”，机器人会解析该指令、识别目标物体并执行抓取任务（Wake et al., 2023c）。核心挑战在于开发稳健的自然语言处理与理解算法——这些算法需能准确解析从直接命令到抽象指令的各类指令，并将其转化为机器人可执行的任务。此外，确保机器人能在不同任务和环境中泛化这些指令，对提升其在实际应用中的通用性和实用性至关重要。在名为“任务与运动规划（Task and Motion Planning）”的机器人框架中，利用语言输入指导机器人任务规划的研究已受到广泛关注（Garrett et al., 2021）。

技能优化（Skill Optimization）近期研究表明LLM在机器人任务规划中效果显著。但任务的优化执行（尤其是涉及抓取等物理交互的任务）需要对环境有更深层次的理解，而非仅解析人类指令。例如，机器人抓取需要精确的接触点（Wake et al., 2023e）和手臂姿态（Sasabuchi et al., 2021），才能高效执行后续动作。这些要素（精确接触点和手臂姿态）对人类而言直观易懂，但通过语言描述却十分困难。尽管互联网规模的VLMs已取得进展，但从场景中捕捉这些细微的间接线索并将其有效转化为机器人技能，仍是一项重大挑战。对此，机器人领域正越来越关注收集增强数据集（例如（Wang et al., 2023d; Padalkar et al., 2023）），或开发从人类演示中直接获取技能的方法（Wake et al., 2021a）。其中，从演示中学习（Learning-from-Demonstration）和模仿学习（Imitation Learning）等框架引领着这些发展，在物理技能优化中发挥着关键作用。

6.2.1 面向机器人技术的LLM/VLM智能体（LLM/VLM Agent for Robotics）

近期研究已证实，LLM/VLM在需与环境中人类交互的机器人智能体领域具有应用潜力。旨在利用最新LLM/VLM技术的研究方向包括：

+   多模态系统（Multimodal Systems）：近期研究正积极聚焦于开发端到端系统，这类系统将最新的LLM和VLM技术作为输入信息的编码器。特别是，目前存在一个显著趋势——对这些基础模型进行改造，使其能够处理多模态信息（Jiang et al., 2022; Brohan et al., 2023, 2022; Li et al., 2023d; Ahn et al., 2022b; Shah et al., 2023b; Li et al., 2023e）。这种改造旨在结合语言指令和视觉线索指导机器人动作，从而实现有效的具身化（embodiment）。

+   任务规划与技能训练（Task Planning and Skill Training）：与端到端系统不同，基于任务与运动规划（Task And Motion Planning, TAMP）的系统会先计算高层任务规划，再通过被称为“技能”的低层机器人控制来实现这些规划。LLM先进的语言处理能力已被证实能够解析指令并将其分解为机器人动作步骤，极大推动了任务规划技术的发展（Ni et al., 2023; Li et al., 2023b; Parakh et al., 2023; Wake et al., 2023c）。在技能训练方面，多项研究探索了LLM/VLM的应用场景：设计奖励函数（Yu et al., 2023a; Katara et al., 2023; Ma et al., 2023）、生成数据以促进策略学习（Kumar et al., 2023; Du et al., 2023），或作为奖励函数的一部分（Sontakke et al., 2023）。结合强化学习（RL）和模仿学习（IL）等训练框架，这些努力将助力高效机器人控制器的开发。

+   现场优化（On-site Optimization）：由于环境条件的意外性和不可预测性，机器人执行长序列任务步骤可能面临困难。因此，机器人领域的一项重大挑战是：通过整合任务规划与实时环境数据，动态调整和优化机器人技能。例如，Ahn等人（2022b）提出了一种方法，可通过视觉信息计算动作的可行性（即可用性，affordance），并与规划任务进行对比。此外，还有一些方法致力于让LLM输出任务步骤的前置条件和后置条件（例如物体状态及其相互关系），以优化执行过程（Zhou et al., 2023c），并检测前置条件错误以便对任务规划进行必要修正（Raman et al., 2023）。这些策略旨在通过整合环境信息，在任务规划或控制器层面调整机器人动作，实现以环境为基础的机器人执行。

+   对话智能体（Conversation Agents）：在构建对话机器人时，LLM可助力机器人与人类进行自然、上下文敏感的交互（Ye et al., 2023a; Wake et al., 2023f）。这些模型能处理并生成模仿人类对话的回应，使机器人能够参与有意义的对话。此外，LLM在估算话语的概念属性（Hensel et al., 2023; Teshima et al., 2022）和情感属性（Zhao et al., 2023; Yang et al., 2023b; Wake et al., 2023d）方面也发挥着重要作用。这些属性有助于理解人类意图并生成有意义的手势，从而提升人机通信的自然性和有效性。

+   导航智能体（Navigation Agents）：机器人导航领域的研究历史悠久，核心聚焦于基于地图的路径规划、同步定位与地图构建（Simultaneous Localization and Mapping, SLAM）等环境地图创建相关方面。这些功能已成为机器人操作系统（Robot Operating System, ROS）等广泛使用的机器人中间件中的标准配置（Guimarães et al., 2016）。

尽管传统导航技术在许多机器人应用中仍占主导地位，但它们通常依赖静态或预创建的地图。近期，随着计算机视觉和自然语言处理等领域的突破，人们对能让机器人在更具挑战性环境中导航的先进技术兴趣日益浓厚。一个代表性任务是物体导航（Chaplot et al., 2020a; Batra et al., 2020; Gervet et al., 2023; Ramakrishnan et al., 2022; Zhang et al., 2021）——机器人使用物体名称而非地图坐标进行导航，这需要将物体名称与环境中的视觉信息关联。此外，近期研究还关注基于基础模型的零样本技术，使机器人能在完全陌生的新环境中导航，即所谓的零样本物体导航（Gadre et al., 2023; Dorbala et al., 2023; Cai et al., 2023）。另外，视觉-语言导航（Vision-Language Navigation, VLN）（Anderson et al., 2018a）也是一项代表性任务，要求智能体在未见过的真实环境中，通过自然语言指令进行导航（Shah et al., 2023a; Zhou et al., 2023a; Dorbala et al., 2022; Liang et al., 2023; Huang et al., 2023b）。VLN解析的是句子而非物体名称（例如“去你左边的卫生间”），因此需要更高的文本解析能力（Wang et al., 2019）。基础模型的出现，通过增强对人类语言指令的理解和对环境信息的视觉解析，推动了这类自适应、即时导航技术的发展。关于代表性VLN研究的更详细说明，见6.2.2节。

![](img/5350cb1076134eb17b6b59d3432f351a.png)

图13：ChatGPT驱动任务规划器的机器人教学系统。该过程包含两个步骤：任务规划——用户通过任务规划器创建动作序列，并根据需要通过反馈调整结果；演示——用户直观演示动作序列，以提供机器人操作所需信息。视觉系统会收集将用于机器人执行的视觉参数。

6.2.2 实验与结果

越来越多的证据表明，近期的VLM和LLM在符号化任务规划（例如“做什么”）方面具备良好能力。但每项任务都需要低层控制策略（例如“怎么做”），才能实现与环境的有效交互。强化学习和模仿学习是通过数据驱动方式学习策略的有效方法，而另一种可行方法是通过现场演示直接从人类获取策略，这种方法被称为“从观察中学习（Learning-from-Observation）”（Wake et al., 2021a; Ikeuchi et al., 0）。本节将介绍一项研究：我们采用ChatGPT进行任务规划，并通过用可用性信息（affordance information）对规划进行参数化来丰富内容，以实现高效且精准的执行（图13）。

该流程由两个模块组成：任务规划和参数化。在任务规划阶段，系统会输入语言指令和工作环境描述。这些指令会与预先定义的机器人动作集及输出规范相结合，整合成完整的提示词提供给ChatGPT，后者随后生成分解后的任务序列及其文本描述（图13；左栏）。值得注意的是，我们采用了少样本（few-shot）方法，这意味着ChatGPT无需针对该任务进行训练——这种方法在适用性上具有优势，因为它无需收集依赖硬件的数据或进行模型训练。此外，输出中的文本描述使用户能够根据需要检查和调整结果，这对安全稳健的操作而言是一项关键特性。图14展示了在VirtualHome（Puig et al., 2018）平台上进行智能体仿真的定性结果。结果显示，该方法能生成合理的任务规划，且在调整输出方面具有灵活性，表明我们的方法具有广泛的适用性。

![](img/d366becad0880ca62547c847a6ad0a92.png)

图14：通过自动生成的反馈调整输出序列的示例。实验使用开源仿真器VirtualHome。给定指令“拿起桌上的馅饼，用炉子加热”，任务规划器会生成VirtualHome中提供的一系列功能序列。若检测到执行错误，任务规划器会根据自动生成的错误信息修正输出。

尽管任务规划器能确保任务序列之间的连贯性，但实际中的成功操作还需要详细参数。例如，抓取类型对于搬运容器且不洒落内部物品至关重要，而这类参数在仿真器中常被忽略（参见图14中抓取馅饼的场景）。因此，在我们的机器人系统中，会要求用户以视觉方式演示每个动作（图13；右栏）。这些任务具有执行所需的预定义参数，由我们的视觉系统从视频中提取（Wake et al., 2021b）。需要说明的是，我们的机器人系统并非设计用于精确复现人类动作（即遥操作（teleoperation）），而是为了应对现实世界中的各种变化条件（如物体位置变化）。因此，从人类演示中提取的参数不包含精确的运动路径，而是包含指导有效环境移动的可用性信息（例如，用于避障的路径点（waypoints）（Wake et al., 2023a）、抓取类型（Wake et al., 2023e）以及上肢姿态（Sasabuchi et al., 2021; Wake et al., 2021a））。对于高自由度机器人而言，上肢姿态至关重要，其设计目的是让与操作机器人共存的人类能预判其姿态。融入可用性信息的任务序列会转化为通过强化学习获得的可复用机器人技能序列，并由机器人执行（Takamatsu et al., 2022）。

通过将[L](https://link.zhihu.com/?target=http%3A//microsoft.github.io/GPT4Vision-Robot-Manipulation-Prompts)LM驱动的任务规划与VLM整合，可将其扩展为更通用的机器人系统。在此，我们展示一个示例：在多模态输入场景下，使用GPT-4V(ision)扩展上述任务规划器（图15）——人类执行需机器人复现的动作。本文仅展示了提示词的部分内容，完整提示词可在microsoft.github.io/GPT4Vision-Robot-Manipulation-Prompts获取。该流程接收演示视频和文本，然后输出机器人动作序列。视觉分析器旨在理解视频中人类执行的动作。我们使用GPT-4V并提供提示词，使其生成符合人际交流风格的文本指令。图16展示了如何通过文本输入让用户对GPT-4V的识别结果提供反馈以进行修正。这一特性旨在提高识别结果的准确性，同时也能增强操作的稳健性。

![](img/76872cb2c13c056ecea900d4dbc3a90f.png)

图15：基于GPT-4V和GPT-4的多模态任务规划器概述。该系统处理视频演示和文本指令，生成供机器人执行的任务规划。

![](img/81a9280d3a3f5e50cf9f2ac84f2fd6c5.png)

图16：视频分析器的输出示例。按固定间隔提取5帧图像输入GPT-4V。我们在6.2.2节中对整个流程进行了描述。

接下来，场景分析器会根据指令和视频数据的第一帧（或环境图像），将预期工作环境整理为文本信息。这些环境信息包括GPT-4V识别出的物体名称列表、物体的可抓取属性，以及物体间的空间关系。尽管这些计算过程在GPT-4V内部属于黑盒，但相关信息会基于GPT-4V的知识及图像/文本输入进行输出。图17展示了场景分析器的示例输出。如图所示，GPT-4V成功筛选出了与操作相关的物体。例如，当人类在桌上移动午餐肉罐头时，桌子会被包含在输出中；而在开启冰箱的任务中，桌子则会被忽略。这些结果表明，场景分析器会结合人类动作对场景信息进行编码。我们通过提示词让GPT-4V解释物体筛选过程的结果及选择依据，实践中发现这种方法能产生合理的输出。最后，任务规划器会根据给定的文本指令和环境信息，输出任务序列（Wake et al., 2023c）。

![](img/9743403014432f5d8a8a415753e4a955.png)

图17：基于GPT-4V的场景分析器输出示例。我们在6.2.2节中对整个流程进行了描述。

机器人导航中的具身智能体（Embodied Agents for Robotics Navigation）

视觉-语言导航（Vision-Language Navigation, VLN）任务是指让具身智能体在真实3D环境中执行自然语言指令。3D环境中的导航能力（Zhu et al., 2017a; Mirowski et al., 2016; Mousavian et al., 2018; Hemachandra et al., 2015）是物理世界中移动智能系统的核心能力。过去几年，学界提出了大量相关任务和评估协议（Savva et al., 2017; Kolve et al., 2017; Song et al., 2017; Xia et al., 2018; Anderson et al., 2018a），（Anderson et al., 2018b）对此进行了总结。VLN（Anderson et al., 2018a）聚焦于真实3D环境中基于语言的导航。为解决VLN任务，（Anderson et al., 2018a）搭建了基于注意力机制的序列到序列基准模型。随后，（Wang et al., 2018）提出了一种混合方法，结合无模型和基于模型的强化学习（RL）以提升模型的泛化能力。最后，（Fried et al., 2018）提出了“说话者-跟随者模型（speaker-follower model）”，该模型采用数据增强、全景动作空间和改进的束搜索（beam search）技术处理VLN任务，在Room-to-Room数据集上达到了当时的最优性能。

在已有研究基础上，我们在（Wang et al., 2019）中提出了用于VLN的强化跨模态匹配（Reinforced Cross-Modal Matching, RCM）方法。RCM模型基于（Fried et al., 2018）的研究构建，但在多个关键方面存在差异：（1）RCM将新型多奖励RL与模仿学习结合用于VLN任务，而（Fried et al., 2018）的“说话者-跟随者模型”仅如（Anderson et al., 2018a）般使用监督学习；（2）RCM的推理导航器（reasoning navigator）会执行跨模态关联，而非对单模态输入采用时间注意力机制；（3）RCM的匹配评判器（matching critic）在架构设计上与“说话者模型（Speaker）”类似，但前者用于为RL和自监督模仿学习（SIL）训练提供循环重构内在奖励，而后者用于为监督学习扩充训练数据。在（Wang et al., 2019）中，我们研究了如何解决该任务的三个关键问题：跨模态关联、病态反馈（ill-posed feedback）和泛化问题。

如图18所示，我们提出了一种新型强化跨模态匹配方法，通过强化学习（RL）从局部和全局层面强化跨模态关联。具体而言，匹配评判器用于提供内在奖励，以促进指令与轨迹间的全局匹配；推理导航器则用于在局部视觉场景中执行跨模态关联。在VLN基准数据集上的评估结果显示，我们的RCM模型在SPL指标上比此前方法显著提升10%，达到了新的最优性能。为提升所学策略的泛化能力，我们进一步提出自监督模仿学习（Self-Supervised Imitation Learning, SIL）方法，通过模仿自身过去的有效决策来探索未见过的环境。结果表明，SIL能学习到更优且更高效的策略，大幅缩小了模型在已见与未见环境间的成功率差距（从30.7%降至11.7%）。此外，在（Wang et al., 2019）中，我们引入了一种用于探索的自监督模仿学习方法，专门解决泛化问题——这是此前研究中未得到充分探讨的问题。与我们同期的研究中，（Thomason et al., 2018; Ke et al., 2019; Ma et al., 2019a,b）从多个角度研究了VLN任务，（Nguyen et al., 2018）提出了VLN任务的一个变体，即让智能体在需要时通过语言求助来寻找物体。需注意的是，我们是首个提出为VLN任务探索未见环境的研究团队。

![](img/b917e3c5e23c0d75e444d3f2deb188d3.png)

图18：视觉-语言导航（VLN）任务的具身智能体演示（Wang et al., 2019）。图中展示了指令、局部视觉场景以及俯视图下的全局轨迹。该智能体无法获取俯视图信息。路径A是遵循指令的演示路径，路径B和路径C是智能体执行的两条不同路径。

### 6.3 医疗健康（Healthcare）

在医疗健康领域，LLM和VLM可作为诊断智能体、患者护理助手，甚至是治疗辅助工具，但它们也面临独特的挑战与责任。AI智能体在改善患者护理和挽救生命方面具有巨大潜力，但与此同时，它们的误用或仓促部署也可能带来同等危险的后果——危及全球成千上万甚至数百万人的生命。本节将探讨AI智能体在医疗健康领域的一些有前景的应用方向，同时也会讨论其面临的部分关键挑战。

诊断智能体（Diagnostic Agents）由于对医疗专家的高需求，以及LLM在患者分诊和诊断方面的潜在作用，将LLM用作医疗聊天机器人进行患者诊断近期受到了广泛关注（Lee et al., 2023）。对话智能体（尤其是那些能向不同患者群体有效传达重要医疗信息的智能体），有望为历史上处于不利地位或边缘化的群体提供公平的医疗服务可及性。此外，全球范围内的医生和医疗系统大多负担过重且资源不足，导致全球数亿人无法获得充分的医疗服务（World Health Organization and World Bank, 2015）。诊断智能体为改善数百万人的医疗健康状况提供了一条极具优势的路径，因为它们可被设计成具备理解多种语言、文化和健康状况的能力。初步研究结果表明，可通过利用大规模网络数据训练出具备医疗健康知识的LLM（Li et al., 2023f）。尽管这是一个令人兴奋的方向，但诊断智能体的应用前景并非没有风险。下一节我们将重点探讨医疗语境下模型“幻觉输出（hallucination）”的风险，以及可能的解决方案。

知识检索智能体（Knowledge Retrieval Agents）在医疗语境中，模型的幻觉输出尤其危险，根据错误的严重程度，甚至可能对患者造成严重伤害或导致死亡。例如，如果患者实际患有某种疾病，却被错误诊断为无病，可能会引发灾难性后果，包括治疗延误或不当，在某些情况下甚至完全缺乏必要的医疗干预。未确诊或误诊病情的严重性可能导致医疗费用上升、治疗周期延长（进一步加重身体负担），在极端情况下还会造成严重伤害甚至死亡。因此，利用智能体更可靠地检索知识（Peng et al., 2023）或以基于检索的方式生成文本（Guu et al., 2020）的方法，是极具前景的研究方向。将诊断智能体与医疗知识检索智能体结合，有望显著减少幻觉输出，同时提高诊断对话智能体响应的质量和准确性。

远程医疗与远程监测（Telemedicine and Remote Monitoring）基于智能体的AI在远程医疗和远程监测领域也具有巨大潜力，可通过改善医疗服务可及性、加强医疗服务提供者与患者之间的沟通、提高频繁医患互动的效率并降低成本来发挥作用（Amjad et al., 2023）。初级保健临床医生需花费大量时间筛选患者的信息、报告和邮件，其中许多内容与他们的工作无关或无需查看。支持智能体在这一领域的应用潜力巨大：它们可帮助分诊来自医生、患者和其他医疗服务提供者的信息，并为各方突出显示重要内容。通过让智能体系统与患者、临床医生及其他AI智能体协同工作，有望彻底变革远程医疗和数字健康行业。

6.3.1 当前医疗健康领域的能力

图像理解（Image understanding）：我们在图19中展示了GPT-4V等现代多模态智能体在医疗健康领域的当前能力与局限性。可以看出，尽管GPT-4V对医院护理涉及的设备和流程具备丰富的内部知识，但在应对用户更具指令性或诊断性的查询时，并非总能给出有效回应。

![](img/7ae11821256a3310457b067c663a043b.png)

图19：在医疗健康图像理解领域GPT-4V示例提示词与响应。从左到右：（1）护士和医生CT扫描图像，（2）不规则心电图（EKG）扫描合成图像，（3）ISIC皮肤病变数据集（Codella et al., 2018）图像。可看出，GPT-4V具备丰富医疗知识，能对医疗图像进行推理。但由于安全训练，它无法对部分医疗图像做出诊断。

视频理解（Video understanding）：我们从两个场景研究了VLM智能体在医疗视频理解方面的性能。首先，探究VLM智能体识别临床空间中重要患者护理活动的能力；其次，探索VLM在超声检查等更专业医疗视频中的应用。具体而言，在图20中，我们展示了GPT-4V在医院护理和医疗视频分析方面的部分当前能力与局限性。在前两个示例中，我们通过提示词让GPT-4V检查视频帧，以检测志愿者患者身上进行的临床床边活动。在最后一个示例中，我们尝试提示GPT-4V评估一段超声心动图视频，但由于GPT-4V的安全训练，它未提供详细响应。（为清晰起见，我们将描述目标活动的文本加粗，并对模型不必要的响应进行了缩写。同时，为保护隐私，我们对人物面部进行了模糊处理。）

![](img/9f150e31e00d5d5083d969386299d369.png)

图20：医疗健康视频理解领域GPT-4V示例提示词与响应。示例视频以2×2网格形式输入，叠加文本标注帧的顺序。

### 6.4 多模态智能体（Multimodal Agents）

视觉与语言理解的融合，对于开发复杂的多模态AI智能体至关重要。这涵盖图像描述生成（image captioning）、视觉问答（visual question answering）、视频语言生成（video language generation）、视频理解等多项任务。本节将深入探讨这些视觉-语言任务，分析其在AI智能体语境下的挑战与机遇。

![](img/b665f446911a7fc0d58da3a989560315.png)

图 21：交互式多模态智能体包含四大核心：交互、语音、视觉与语言，1）交互服务助力搭建自动化动作、认知与决策的统一平台；2）音频服务将音频与语音处理集成到应用程序与服务中；3）视觉服务识别并分析图像、视频及数字墨迹中的内容；4）语言服务从结构化与非结构化文本中提取语义。

6.4.1 图像-语言理解与生成（Image-Language Understanding and Generation）

图像-语言理解任务涉及通过语言解读图像中的视觉内容，并生成相关的语言描述。这一任务对开发能以更类人方式与世界交互的AI智能体至关重要。其中最热门的任务包括图像描述生成（Lin et al., 2014; Sharma et al., 2018; Young et al., 2014; Krishna et al., 2016）、指代表达（referring expression）（Yu et al., 2016; Karpathy et al., 2014）以及视觉问答（VQA）（Antol et al., 2015; Ren et al., 2015; Singh et al., 2019）。

近年来，学界还提出了OKVQA（Marino et al., 2019）、KB-VQA（Wang et al., 2015）、FVQA（Wang et al., 2017）、WebQA（Chang et al., 2021）等知识密集型视觉问答（VQA）任务。多模态智能体应能够识别图像中的物体、理解物体间的空间关系、生成关于场景的准确描述语句，并运用推理能力处理知识密集型视觉推理任务。这不仅需要物体识别能力，还需深入理解空间关系、视觉语义，以及结合世界知识将这些视觉元素映射到语言结构的能力。

6.4.2 视频与语言理解与生成（Video and Language Understanding and Generation）

视频语言生成（Video-language generation）：视频描述生成或视频故事生成任务，是为视频帧序列生成连贯的句子序列。受递归大型基础模型在视频与语言任务中成功应用的启发，基于智能体驱动的增强模型变体在视频语言生成任务中展现出良好效果。核心挑战在于：神经编码器-解码器模型的出色性能在视觉故事生成任务中泛化性较差，因为该任务不仅要求理解每帧图像的内容，还需掌握不同帧之间的关联。该领域的一个重要目标是构建“智能体感知型文本合成模型”，能够高效编码帧序列，并生成主题连贯的多句段落。

视频理解（Video Understanding）：视频理解将图像理解的范围扩展到动态视觉内容，涉及对视频帧序列的解读与推理，通常还需结合伴随的音频或文本信息。智能体应能与视觉、文本、音频等多种模态交互，以体现其对视频内容的高级理解能力。该领域任务包括视频描述生成、视频问答、动作识别（activity recognition）等。视频理解面临的挑战多样，包括视觉与语言内容的时间对齐（temporal alignment）、长帧序列的处理，以及对随时间展开的复杂活动的解读。在音频方面，智能体需处理语音、背景噪音、音乐和语调，以理解视频内容的氛围、场景和细节。

已有研究主要利用网络上现有的视频-语言训练数据构建视频基础模型（Li et al., 2020, 2021b; Fu et al., 2022; Bain et al., 2021; Zellers et al., 2021, 2022; Fu et al., 2023）。然而，由于这些数据集规模有限且往往存在不一致性，支持此类训练流程和功能存在难度。视频基础模型通常采用掩码和对比预训练目标（masked and contrastive pretraining objectives）进行训练，随后在特定任务上微调。尽管这些模型在多模态基准测试中表现出色，但由于依赖基于嘈杂音频转录构建的有限视频-文本数据，它们在动作识别等纯视频任务中面临困难。这一局限还导致其缺乏大语言模型普遍具备的稳健性和细粒度推理能力。

其他方法与图像-语言理解领域类似，借助大语言模型强大的推理能力和广博知识，来提升视频解读的各个方面。ChatGPT、GPT-4等纯语言模型，或GPT-4V等图像-语言模型，将音频、视频和语言模态视为独立的可解读输入数据类型，并将智能体定位为强大的开源模型，从而简化了视频理解任务。例如，（Huang et al., 2023c; Li et al., 2023g）通过开源视觉分类/检测/描述模型将视频内容文本化，将视频理解转化为自然语言处理（NLP）问答任务。（Lin et al., 2023）将GPT-4V与视觉、音频和语音领域的专用工具整合，以处理复杂的视频理解任务（如长视频中角色动作与行为的脚本生成）。

探索从大型模型生成规模化数据集，然后在生成数据上进行视觉指令微调（visual instruction tuning）（Liu et al., 2023c; Li et al., 2023c; Zhu et al., 2023）。随后，大量音频、语音和视觉专业感知模型被用于将视频转化为语言形式：通过自动语音识别（ASR）工具转录语音，借助各类标注、关联和描述模型生成视频描述及相关数据（Li et al., 2023g; Maaz et al., 2023; Chen et al., 2023; Wang et al., 2023f）。这些技术表明，在生成数据集上对视频-语言模型进行指令微调，可能提升模型的视频推理和沟通能力。

6.4.3 实验与结果

+   知识密集型模型（Knowledge-Intensive Models）：如INK（Park et al., 2022）和KAT（Gui et al., 2022a）中所介绍，这是一项密集神经知识任务（intensive neural knowledge task），整合了人类标注的必要知识，以支持知识密集型检索任务。

+   多模态智能体（Multimodal-Agents）：学界对Chameleon（Lu et al., 2023）和MM-React（Yang et al., 2023c）等多模态语言模型的研究兴趣日益浓厚。

+   视觉指令微调（Visual Instruction Tuning）：VCL（Gui et al., 2022b）、Mini-GPT4（Zhu et al., 2023）、MPLUG-OWL（Ye et al., 2023b）、LSKD（Park et al., 2023c）等模型会生成图像级别的指令微调数据集。

知识密集型智能体（Knowledge-Intensive Agent）如图22和图23所示，基于知识的视觉问答（Knowledge-based visual question answering）和视觉-语言检索任务（vision-language retrieval tasks）是多模态机器学习中的挑战性任务，这类任务需要图像内容之外的外部知识。近期关于大规模Transformer的研究主要聚焦于最大化模型参数的信息存储效率，而本研究方向则探索了另一个层面：多模态Transformer能否在决策过程中使用显式知识（explicit knowledge）。基于Transformer的预训练方法在隐式学习（implicit learning）多模态知识表示方面已取得显著成功，但传统方法（以单模态为主）虽对知识检索及后续答案预测进行了研究，却引发了关于检索知识的质量与相关性、以及如何整合隐式知识（implicit knowledge）与显式知识进行推理等问题。

为解决这些问题，我们提出了知识增强Transformer（Knowledge Augmented Transformer, KAT）。该模型在2022年OK-VQA开放域多模态任务上表现优于其他模型6%。KAT采用编码器-解码器结构，结合了来自GPT-3的隐式知识和来自网站的显式知识，能够在答案生成过程中结合两种知识进行协同推理。此外，融入显式知识还提升了模型预测结果的可解释性。相关代码和预训练模型可在https://github.com/guilk/KAT获取。

![](img/a3d33cc607af4691169eac7ba50a315c.png)

图22：密集神经知识（Intensive Neural Knowledge, INK）任务示例（Park et al., 2022）。该任务利用知识从一组文本候选中识别与图像相关的文本。我们的任务涉及利用从网络和人类标注知识中检索到的视觉和文本知识。

![](img/cf6b78e6ab135ba296f7bb40c762cf09.png)

图23：KAT模型（Gui et al., 2022a）的工作流程。该模型通过基于对比学习的模块从显式知识库中检索知识条目，并利用GPT-3检索带支持证据的隐式知识。知识整合过程由相应的编码器Transformer处理，并通过端到端训练与推理模块、解码器Transformer协同工作，以生成答案。

视觉-语言Transformer智能体（Vision-language Transformer Agent）接下来，我们介绍“基于描述文本训练视觉-语言Transformer（Training Vision-Language Transformers from Captions, VLC）”模型（Gui et al., 2022b）。这是一种仅使用图像-描述对进行预训练的Transformer。与依赖目标检测器或有监督CNN/ViT网络的其他方法不同，VLC仅通过简单的线性投影层（linear projection layer）处理图像嵌入（image embeddings），却在各类视觉-语言任务上取得了具有竞争力的结果。

图24为VLC模型的整体架构（Gui et al., 2022b）。该模型包含三个模块：（1）模态特异性投影（Modality-specific projection）：采用简单的线性投影对分块图像进行嵌入，通过词嵌入层对分词后的文本进行嵌入；（2）多模态编码器（Multi-modal encoder）：以12层ViT（Dosovitskiy et al., 2021）作为骨干网络，该ViT由MAE（He et al., 2022）（基于无标签ImageNet-1K）初始化；（3）任务专用解码器（Task-specific decoder）：通过掩码图像/语言建模和图像-文本匹配学习多模态表示（这些方法仅在预训练阶段使用）。我们使用2层MLP对多模态编码器进行微调，以适应下游任务。重要的是，我们发现掩码图像建模目标在第二阶段预训练的整个过程中都至关重要，而非仅用于视觉Transformer的初始化。

通过深入分析，我们探索了VLC作为视觉-语言Transformer智能体的潜力。例如，研究表明VLC的视觉表示在ImageNet-1K分类任务中效果显著，且可视化结果证实VLC能准确将图像补丁与对应的文本tokens匹配。随着训练数据增加，模型性能展现出良好的可扩展性，这凸显了开发大规模、弱监督、开放域视觉-语言模型的广阔前景。

![](img/72dcf8fb576a846ea0d872c6561b986b.png)

图24：VLC模型的整体架构（Gui et al., 2022b）

### 6.5 视频-语言实验

为探究将预训练图像-LLM转换用于视频理解的可行性，我们对InstructBLIP（Dai et al., 2023）进行时间维度扩展与微调，以适配视频描述生成任务。具体而言，我们采用与《Frozen in Time》（Bain et al., 2021）相同的分块时空注意力机制（divided space-time attention scheme），对InstructBLIP的视觉编码器（EVA-CLIP-G（Sun et al., 2023b））进行扩展；训练期间，冻结Q-former和LLM（Flan-T5-XL（Chung et al., 2022））。在描述生成训练过程中，我们冻结视觉编码器的所有空间层（spatial layers），仅解冻时间层（temporal layers）。这使得模型能够以图像和视频作为输入（匹配InstructBLIP的图像级性能）。

我们在WebVid10M（Bain et al., 2021）的一个包含500万条视频-描述对的子集上进行训练，并在图25中可视化了两个示例输出。然而，现有智能体无法完全理解视频内容中精确、细粒度的视觉细节。视觉指令微调方法也存在类似局限，它们缺乏人类级别的通用感知能力——这仍是多模态模型与智能体需要解决的问题。

![](img/d67a1ddc47919981b3cbc5c4ab8472ab.png)

图25：使用经视频微调的InstructBLIP变体（方法详见6.5节）时的示例提示词与响应。我们的模型能够生成描述场景的长文本响应，并能回答与视频中事件时间顺序相关的问题。

如图25所示，经过指令微调的模型在准确总结视频中可见动作方面表现出潜力，能有效识别“人坐在长椅上”等动作。但它们有时会添加错误细节，例如“人对着镜头微笑”，这表明模型在捕捉对话主题或视频氛围（这些对人类观察者而言显而易见的元素）方面存在不足。这一不足凸显了另一个关键局限：模型未纳入音频和语音模态，而这些模态本可通过提供上下文来丰富视频理解，助力更准确的解读并避免此类误判。要弥补这一差距，需对现有模态进行整体整合，使多模态智能体达到接近人类感知的理解水平，确保以全多模态方式解读视频。

基于GPT-4V的音视频-语言智能体（Audio-Video-Language Agents with GPT-4V）

随后，我们参照（Lin et al., 2023）中概述的方法，评估GPT-4V作为整合视觉、音频和语音的多模态智能体对视频进行细致精准理解的能力。图26展示了不同视频智能体在视频总结任务上的性能对比。经视频指令微调的模型（Li et al., 2023g）能提供准确内容，但在完整性和细节上存在不足，例如遗漏了“用扫帚柄有条理地测量树高”等具体动作。

![](img/b1dee147857a79a3fae6e6ee15f3cd31.png)

图26：6.5节所述的音频多模态智能体。幻觉输出内容用红色突出显示。我们使用GPT-4V生成三种总结：1）基于视频帧的视频对话总结；2）基于帧描述的视频总结；3）基于帧描述和音频信息的视频总结。

为提升视频描述的准确性，我们使用GPT-4V生成帧描述，同时通过OpenAI Whisper模型获取音频及其转录文本。之后，我们分别以“仅帧描述”和“帧描述+音频转录文本”为输入，提示GPT-4V生成视频总结。初步结果显示，仅使用帧描述时，模型可能生成虚构事件，例如在第三段中出现“人咬住棍子”的描述。这些不准确信息会延续到视频总结中，出现“他俏皮地将棍子水平拿着并咬了下去”等表述。若没有音频输入，智能体无法纠正这些描述错误，导致生成内容虽语义正确但存在视觉误导性。

然而，当向智能体提供音频转录文本后，它能够准确描述内容，甚至捕捉到“将扫帚柄垂直于身体握持并向下旋转”等详细肢体动作。这种细节水平的描述信息量显著更高，能让观者更清晰地理解视频的目的和关键细节。这些发现凸显了整合音频、视频和语言交互对开发高质量多模态智能体的重要性，而GPT-4V则成为此类高级多模态理解与交互的潜在基础模型。

![](img/caaa1feb4ea2bb4cd1fe92a27f7f76cf.png)

图27：整合视觉、音频和文本模态用于视频理解的交互式多模态智能体。我们的流程通过挖掘难例负样本幻觉输出，为VideoAnalytica挑战赛生成难题。关于交互式音视频-语言智能体数据集的更多细节，详见9.2节。

基于GPT-4V的具身多模态智能体（Embodied Multi-modal Agents with GPT-4V）

如图27所示，我们主要通过StackOverflow获取初始问题，随后使用“Bing搜索”API检索与该问题对应的相关视频和音频。接下来，主要借助GPT-4V获取相关文本信息和高级视频描述；另一方面，通过自动语音识别（ASR）将关键帧音频转换为关键帧的低级片段描述。最后，利用GPT-4V生成具有迷惑性的“幻觉输出（hallucinations）”，作为视频问答任务的难例负样本查询。我们支持对视频当前帧进行交互和问答，也支持对视频整体进行高级描述总结。在推理阶段，我们还通过网络搜索结合外部知识信息，以提升回答能力。

GPT-4V的核心提示词信息如下：

GPT-4V需作为助手，为能听到视频声音但无法看到画面的视障人士提供视频中描述性、信息性且全面详尽的细节。你的工作是综合给定的标注信息，生成高质量、内容详实的视频描述，并以JSON格式输出。

具体而言，将向GPT-4V提供用于搜索视频的原始查询词、视频标题、视频描述、音频转录文本，以及可能包含的视频特定时间点的噪声描述。同一视频的不同片段标注格式为“[时间起点 - 时间终点（以秒为单位）]‘文本描述’”。请综合利用转录文本和所有描述信息，推理视频中可能发生的具体细节和视觉演示内容。GPT-4V需根据需要合并或拆分时间戳，以实现最佳的视频片段划分。

对GPT-4V输出的要求：

1.  面向动作的描述：优先呈现音频所暗示的合理动作、运动和肢体演示，用动态视觉线索丰富叙述内容。

1.  完整视频覆盖：提供连贯且一致的音频描述体验，覆盖视频时长内的每一个时刻，确保无内容遗漏。

1.  简洁片段划分：将描述内容拆分为聚焦且简洁的片段，每个片段1-2句话，在有效传达视觉动作的同时避免细节冗余。

1.  音视频上下文融合：将语音音频内容与推断的视觉元素无缝融合，形成能反映屏幕上可能发生活动的叙述。

1.  合理且具想象力的推测：在描述中融入与音频相符的富有创意但合理的视觉细节，提升场景理解度。

1.  准确时间码对应：使描述片段与对应的时间码对齐，确保推测的视觉细节与音频叙述的时间线同步。

1.  自信的叙述表达：以肯定的语气呈现描述内容，仿佛推测的视觉画面正在发生，让听者产生信任感。

1.  排除不合理细节：不描述与所提供的音频和视觉信息所建立的上下文不符的物体或事件。

最终输出需采用JSON格式，包含一个字典列表，每个字典详细描述视频的一个片段：

[ ‘start’: <开始时间（秒）>, ‘end’: <结束时间（秒）>, ‘text’: “<此处为你的详细单句音视频描述内容>” ]

关于多项选择题创建：我们的任务是生成用于视频到文本检索任务的多项选择题，这类题目通过查看标题和阅读音频转录文本即可轻松解答。为此，我们将获得用于获取视频的原始查询词、视频描述、音频转录文本，以及可能包含的视频特定时间点的噪声描述。音频转录文本格式：-[开始-结束时间（秒）]“转录内容”

+   噪声描述格式：- [时间（秒）]“描述内容”

请GPT-4V生成4个查询词，其中第一个查询词（主查询词）与视频内容一致，另外三个（负查询词）与主查询词存在细微差异。选择主查询词不应仅通过聆听音频转录文本实现（例如，原始查询词文本包含在音频转录文本中）。负查询词需与视频内容密切相关但不完全一致，需通过对视频的视觉理解来区分。例如，对语义进行细微修改，使得必须观看视频而非仅聆听音频才能选出原始查询词。将4个查询词整理为类似描述的语句，第一个为改写后的原始查询词。

请逐步思考如何利用视频中的信息生成负查询词，并说明这些负查询词为何不正确但仍具有迷惑性（需对视频有细致理解才能区分），以及人类为何不会误将负查询词当作原始查询词。

最后，按以下格式呈现分析内容和4个查询词，无需说明如何改写原始查询词：

+   视频分析：xxx

+   查询词：[query1, query2, query3, query4]

+   说明：xxx

### 6.6 面向自然语言处理的智能体

6.6.1 LLM智能体（LLM agent）

数十年来，识别任务指令并采取行动一直是交互式AI和自然语言处理领域的核心挑战。随着深度学习领域的近期进展，学界对联合研究这些领域以改善人机协作的兴趣日益浓厚。我们确定了（其中）三个具体方向，以提升基于语言的智能体性能：

+   工具使用与知识库查询（Tool use and querying from knowledge bases）：该方向强调将外部知识库、网络搜索或其他实用工具整合到AI智能体推理过程中的重要性。通过利用来自不同来源的结构化和非结构化数据，智能体能够增强自身理解能力，提供更准确且符合上下文的响应。此外，这还能培养智能体在面对陌生场景或查询时主动获取信息的能力，确保响应更全面、更具依据。相关示例包括Toolformer（Schick et al., 2023）和Retrieve What You Need（Wang et al., 2023g）。

+   增强智能体推理与规划能力（Improved agent reasoning and planning）：提升智能体的推理与规划能力是实现高效人机协作的关键。这需要开发能够理解复杂指令、推断用户意图并预测潜在未来场景的模型。实现方式包括让智能体反思过往动作与失败（如ReAct（Yao et al., 2023a）），或将智能体的思维过程构建为一种搜索形式（Yao et al., 2023b）。通过模拟不同结果并评估各类动作的影响，智能体能够做出更具依据、符合上下文的决策。

+   整合系统与人类反馈（Incorporating system and human feedback）：AI智能体通常在两种主要场景下运行：一是环境会为其动作有效性提供明确信号（系统反馈），二是与能提供口头评价的人类协作（人类反馈）。该方向强调需要自适应学习机制，使智能体能够优化策略并纠正错误，例如AutoGen（Wu et al., 2023）。从多种反馈源持续学习和适应的能力，确保智能体始终对用户需求有帮助且保持对齐。

6.6.2 通用LLM智能体（General LLM agent）

数十年来，识别和理解智能体内容与自然语言一直是交互式AI和自然语言处理领域的核心挑战。随着深度学习的近期进展，学界对联合研究这两个领域的兴趣不断增加，以期深入理解智能体规划或用于知识推理与自然语言生成的人类反馈。这些都是许多人机交互智能体的关键组成部分，例如“AutoGen”（Wu et al., 2023）和“Retrieve What You Need”（Wang et al., 2023g）。

6.6.3 遵循指令的LLM智能体（Instruction-following LLM agents）

此外，构建可通过训练有效遵循人类指令的LLM智能体已成为重要研究方向。早期模型通过一种名为基于人类反馈的强化学习（Reinforcement Learning with Human Feedback, RLHF） 的方法，利用人类反馈训练代理奖励模型（proxy reward model）来模拟人类偏好（Ouyang et al., 2022）。这一过程催生了InstructGPT和ChatGPT等模型。为了更高效地训练遵循指令的LLM智能体（无需人类标签），研究人员开发了一种更高效的指令微调方法：直接使用指令/响应对训练LLM智能体，这些配对既可以由人类生成（如Dolly 2.0 6），也可以由LLM自动生成（如Alpaca（Taori et al., 2023））。我们在图28中展示了Alpaca的整体训练流程。

![](img/3389022930bd4f1caa825744cf05ac59.png)

图28：Alpaca模型的训练流程（Taori et al., 2023）。从整体来看，研究人员利用现有LLM，从少量种子任务中生成大量指令遵循示例；这些生成的指令遵循示例随后被用于对权重可获取的LLM进行指令微调。

6.6.4 实验与结果

尽管对话式和自反馈系统的应用日益广泛，但这类AI在利用自身隐式知识生成事实性正确的响应方面表现仍不理想，因此在推理阶段往往需要借助网络搜索、知识检索机制等外部工具来补充响应内容。解决这一问题将有助于在众多实际应用中为用户创造更具吸引力的体验。在社交对话中（如Instagram、Facebook等社交媒体平台上的对话），或在问答网站（如Ask、Quora）中，人们通常通过一系列评论互动，以及搜索与讨论相关的信息和知识来与他人交流。因此，在这一语境下生成对话轮次的任务，并非简单以传统NLP模型和任务为基础，而是要通过智能体模拟知识搜索与获取的智能行为来生成对话（Peng et al., 2023）。通过这种方式，面向NLP任务的智能体扩展了任务描述，并在对话过程中增加显式的知识搜索与检索步骤，从而提升响应的可解释性。在对话中整合这些网络搜索和检索智能体作为反馈，将有助于深化人机之间的社交互动（Wang et al., 2023e）。

如图29所示，我们提出了一种适用于Transformer语言模型的新型建模范式——作为一类NLP智能体，它能从输入文本中检测并提取重要的逻辑结构和信息，然后通过精心设计的多层层级逻辑投影（hierarchical logical projections） 将这些信息整合到输入嵌入中，从而将逻辑结构注入预训练语言模型。Wang等人（2023e）提出了一种新颖方法，通过结合逻辑检测、逻辑映射和层级逻辑投影，为Transformer语言模型构建具有逻辑感知能力的输入嵌入，进而开发出相应的新型建模范式。该范式可将所有现有Transformer语言模型升级为逻辑Transformer（logical transformers），持续提升其性能。所提出的逻辑Transformer智能体通过更深入理解文本的逻辑结构，在各项基准测试中均表现出优于基础Transformer模型的性能。对人类用户而言，通过智能体协调对话与信息检索，正是这些方面对于开展有意义且有趣的对话更为重要。深入自然语言处理领域，本主题将探讨如何提升LLM的智能体特性，使其更适合各类以语言为核心的任务，以及这一过程中的进展与挑战。

![](img/0c92a2caf1c388b5906d68fdab49e2fe.png)

图29：逻辑Transformer智能体模型（Wang et al., 2023e）。我们将逻辑推理模块整合到基于Transformer的抽象摘要模型中，使该逻辑智能体具备对文本和对话逻辑进行推理的能力，从而生成质量更高的抽象摘要并减少事实性错误。

开放域问答（QA）系统通常遵循“检索-阅读（retrieve-then-read）”范式：首先由检索器（retriever）从大型语料库中检索相关段落，然后由阅读器（reader）根据检索到的段落和原始问题生成答案。在Wang等人（2023g）的研究中，我们提出了一种简单新颖的互学习框架（mutual learning framework），通过一个名为“知识选择器智能体（knowledge selector agent）”的中间模块（采用强化学习训练）来提升“检索-阅读”式模型的性能。该框架将细粒度知识选择器融入“检索-阅读”范式，其目标是构建一个保留问题相关信息的小型段落子集。如图30所示，知识选择器智能体作为新型互学习框架的组成部分进行训练，该框架会迭代训练知识选择器和阅读器。我们采用一种简单新颖的方法，利用策略梯度（policy gradients）优化知识选择器智能体，并借助阅读器的反馈训练它选择规模小但信息量大的段落集。这种方法无需暴力搜索或人工设计启发式规则，也不需要任何带标注的查询-文档对作为监督信息。实验表明，通过迭代训练阅读器和知识选择器智能体，模型在部分公开开放域问答基准测试中的预测性能得到了提升。

![](img/bf8ee9d46a594dd12d359fcc731be0e3.png)

图30：所提出的NLP智能体（Wang et al., 2023g）互学习框架架构。在每个训练周期中，阶段1和阶段2交替执行：阶段1固定阅读器模型参数，仅更新知识选择器的权重；反之，阶段2调整阅读器模型参数，同时冻结知识选择器的权重。

---

[【A](https://zhuanlan.zhihu.com/p/1963911695043897137)I进展精译】 AI AGENT综述：多模态交互新态势（一）｜李飞飞 2024｜AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

[【A](https://zhuanlan.zhihu.com/p/1963932571718104887)I进展精译】 AI AGENT综述：多模态交互新态势（二）｜李飞飞 2024｜AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

[【A](https://zhuanlan.zhihu.com/p/1963929090315753256)I进展精译】 AI AGENT综述：多模态交互新态势（三）｜李飞飞 2024｜AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

[【A](https://zhuanlan.zhihu.com/p/1963929824440607906)I进展精译】 AI AGENT综述：多模态交互新态势（四）｜李飞飞 2024｜AGENT AI: SURVEYING THE HORIZONS OF MULTIMODAL INTERACTION

[【A](https://zhuanlan.zhihu.com/p/1964993295185840031)I进展精译】 AI AGENT综述：多模态交互新态势（五、附录）｜李飞飞 2024｜AGENT AI: THE HORIZONS OF MULTIMODAL INTERACTION

[【A](https://zhuanlan.zhihu.com/p/1965000505643242317)I进展精译】 AI AGENT综述：多模态交互新态势（六、参考文献）｜李飞飞 2024｜AGENT AI: THE HORIZONS OF MULTIMODAL INTERACTION

编辑于 2025-10-24 10:48・北京