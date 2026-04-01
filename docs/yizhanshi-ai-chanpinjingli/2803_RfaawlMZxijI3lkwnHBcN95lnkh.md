# 3🈷️23日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IcEednzWNoLY4kxCVG3cXaXNnme](https://v11enp9ok1h.feishu.cn/docx/IcEednzWNoLY4kxCVG3cXaXNnme)

> 🔔AI情报助手早报「03月23日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> OpenAI推出GPT-5.4 mini...

>

+   OpenAI发布GPT-5.4 mini/nano轻量模型，优化速度成本，支持多模态与工具调用，API定价低（mini$0.75/百万token，nano$0.20）。

+   MiniMax推出M2.7模型，具备自我演化能力，可自主优化工具包，在SWE-Pro等测试接近顶级，机器学习竞赛获多奖。

+   小米发布MiMo-V2系列三款模型：Pro（智能体核心，全球综合第8）、Omni（全模态理解）、TTS（情感语音合成）。

+   OpenAI轻量模型服务高吞吐场景，MiniMax聚焦自我演化，小米打造智能体能力闭环。

+   各模型在基准测试表现优异，如GPT-5.4 mini SWE-Bench Pro达54.4%，M2.7开源模型排名第一。

> 关键词：GPT-5.4 mini/nano，MiniMax M2.7，小米MiMo-V2系列，大模型，智能体

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/jk7LL8U0F04B5rBfdXpNKw)

+   OpenAI GPT-5.4 mini/nano介绍

+   [MiniMax M2.7发布](https://www.minimax.io/news/minimax-m27-en)

+   [小米MiMo-V2系列](https://mimo.xiaomi.com/)

> 2️⃣

> 🤖

> [聊聊微信的Agent野心](https://mp.weixin.qq.com/s/te-n9a4yAnwF5b6ioatR1A)

>

+   2026年3月22日微信上线ClawBot插件，作为连接外部AI Agent的官方管道，入口类似文件传输助手。

+   与现有AI入口“元宝”不同，ClawBot仅提供微信内交互通道，不限制后端运行的AI模型（如OpenClaw、Claude等）。

+   该插件打破微信以往自建生态模式，首次开放官方管道允许第三方AI接入，遏制野路子泛滥。

+   微信服务协议明确：有权管控第三方AI服务类型、频率及内容安全，核心聚焦入口/渠道控制权。

+   微信此举因OpenClaw等野生接入方案迭代快，旨在掌握消息链路，凸显“入口比自身AI更重要”的战略。

> 关键词：微信ClawBot，AI Agent管道，第三方AI接入，微信生态开放，AI入口战略

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/te-n9a4yAnwF5b6ioatR1A)

> 3️⃣

> 🤖

> PRDBench：Code Agent 评测范式，正在被重写

>

+   上海交大等团队联合提出PRDBench，针对Code Agent从“写代码”到“做项目”的能力升级，重构以PRD为入口的项目级评测范式。

+   现有基准（如HumanEval）失效：聚焦局部函数正确性，忽略真实开发的需求理解、跨文件协作等项目级能力。

+   PRDBench含50个真实Python项目（20领域），评测维度扩展为功能正确性、交互行为、产物一致性三类。

+   创新采用agent-driven数据生产（agent生成+人工监督），并训练专用模型PRDJudge提升评测稳定性。

+   实验显示Code Agent项目级能力仍有差距，能力取决于模型×框架×工程约束的组合。

> 关键词：PRDBench，Code Agent，AI评测，软件工程，评测范式

>

> 🔗 文中引用的链接

+   [Paper](https://arxiv.org/abs/2510.24358)

+   [GitHub](https://github.com/AGI-Eval-Official/PRDBench)

+   [Huggingface](https://huggingface.co/datasets/AGI-Eval/PRDbench)

> 4️⃣

> 🤖

> 在线RL与离线RL的本质区别是什么，在LLM训练中又如何权衡？

>

+   在线RL（PPO/GRPO）实时采样当前策略rollout，无分布偏移但计算开销高；离线RL（DPO）用固定数据集，数据复用率高却存在分布偏移问题。

+   数学推理任务中在线RL（GRPO）优势明显（DeepSeek-R1实验超DPO5-10%），通用对话任务离线DPO表现好且成本更低。

+   Online DPO结合两者优势，比离线DPO在MT-Bench高5-8%，成本仅PPO30-40%；Iterative RLHF是LLaMA系列等工业级对齐标准。

+   任务选择决策树：可验证奖励任务选在线RL，主观任务有偏好数据选DPO，否则用Online DPO，预算充足选Iterative RLHF。

+   工程建议：资源有限选DPO，推理任务用在线RL，混合方法是主流对齐方案，Online DPO是性价比最优折衷。

> 关键词：在线RL，离线RL，LLM训练，DPO，PPO

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/uwBQuU6JE_1dWnwfjZUzcQ)

+   [知乎原文](https://zhuanlan.zhihu.com/p/2016080986690041206)

+   [DPO论文](https://arxiv.org/abs/2305.18290)

+   [RAFT论文](https://arxiv.org/abs/2304.06767)

+   [Online DPO论文](https://arxiv.org/abs/2402.04792)

+   [LLaMA 2论文](https://arxiv.org/abs/2307.09288)

> 5️⃣

> 🤖

> 13 | OpenClaw 接入微信：个人微信原生接入，终于来了

>

+   微信灰度测试ClawBot插件，支持个人微信原生接入OpenClaw，iPhone用户升级微信8.0.70后，一行命令+扫码即可5分钟完成配置。

+   提供三种接入方案：原生ClawBot（最优）、企业微信+微信插件（通用稳定）、第三方协议（有封号风险仅供研究）。

+   展示5个微信内AI协同场景：通勤读文关联知识库、手机审合同、爬楼整理待办、写周报、改PPT，全程无需切换应用。

+   指出4个常见踩坑：配置顺序（先OpenClaw后企业微信）、防火墙端口未放通、个人微信未关联企业插件、参数混淆。

+   核心价值：将AI助手融入日常微信沟通，降低使用门槛，从企业级工具变为个人随时可用的生活/工作助手。

> 关键词：OpenClaw、微信接入、AI助手、原生ClawBot、场景化协同

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/F3S7fDPx9KS4q44Qeemc0A)

> 6️⃣

> 🤖

> 劲爆！个人微信官方接入龙虾了【喂饭级教程...

>

+   2026年3月22日消息，微信官方正式支持接入开源原生龙虾（ClawBot），无封号风险。

+   接入需3步：升级微信至8.0.70版本，安装龙虾微信插件，终端执行指定命令后扫码登录。

+   目前支持功能：图片上传识别（需视觉模型如kimi-k2.5）、语音交流，文件识别暂存问题。

+   作者推测微信因其他IM均支持龙虾接入而跟进，期待后续开放群聊等功能。

+   龙虾官方安装参考文档为：

+   [h](https://docs.openclaw.ai/zh-CN/start/getting-started)ttps://docs.openclaw.ai/zh-CN/start/getting-started

+   。

> 关键词：微信接入龙虾，ClawBot，AI工具，微信插件，龙虾安装教程

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/mpf7EidCn_p6vvIOR2om_Q)

+   [龙虾官方安装文档](https://docs.openclaw.ai/zh-CN/start/getting-started)