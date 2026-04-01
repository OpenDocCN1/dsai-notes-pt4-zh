# 3🈷️9日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HoEDdfSFeoBcTHx2wlHcCLJonyf](https://v11enp9ok1h.feishu.cn/docx/HoEDdfSFeoBcTHx2wlHcCLJonyf)

> 🔔AI情报助手日报「03月09日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> Karpathy开启"自动研究"实验：AI开始自己写代码训练自己

>

+   Andrej Karpathy开源autoresearch项目（630行代码），实现AI代理自动完成研究循环：人类写指令→AI改训练代码→跑实验→评估迭代，每小时约12次实验（5分钟/次）。

+   项目核心3文件：prepare.py（固定基础设施）、train.py（AI唯一可修改的训练代码）、program.md（人类研究指令），用git分支管理实验迭代。

+   实测8个H100一晚上跑276次实验，29次有改进，AI还优化随机种子（从经典42改为137）。

+   存在争议：批评者认为是指标驱动的爬山算法（非自主科学研究），支持者认可固定时间窗口强制架构权衡的设计。

+   研究瓶颈转变为“提问题”，技能转向设计研究策略，项目MIT开源（支持NVIDIA GPU，已有macOS fork）。

> 关键词：Karpathy，autoresearch，AI自动研究，机器学习实验，开源项目

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/7u58bVQZAmVwrlA6S-NhtA)

+   autoresearch项目地址

> 2️⃣

> 🤖

> [200万样本、200任务：伯克利、NTU、CMU、斯坦福、约翰](https://mp.weixin.qq.com/s/LHpC12oEzljc7cMz-kGXDQ)霍普金斯等机构联合推进“可验证视频推理”

>

+   多顶尖机构联合提出VBVR套件，以200任务、201.5万图像、100.75万视频构建可训练/验证的视频推理基础设施。

+   VBVR含按5类认知能力设计的数据集（可参数化扩展）及规则可验证评测工具（对齐人类偏好）。

+   微调后的VBVR-Wan2.2基准提升84.6%，多维度超主流专有模型，揭示能力间复杂相关性。

+   数据扩展提升同域/异域泛化，但存在天花板，需架构改造（如状态跟踪、自校正）。

+   强调“可控性先于推理”，模型学会按规则执行，但长时程任务仍有失败模式。

> 关键词：视频推理、VBVR、可验证评测、认知能力、规模化研究

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/LHpC12oEzljc7cMz-kGXDQ)

+   [论文链接](https://huggingface.co/papers/2602.20159)

+   [PaperScope链接](https://www.paperscope.ai/hf/2602.20159)

> 3️⃣

> 🤖

> OpenClaw免费小白安装教程来了！养...

>

+   教程介绍国产开源多模型AI桌面客户端Cherry Studio，支持一键安装OpenClaw，全程免费、零基础可上手，10-20分钟即可拥有持久化AI伙伴🦞。

+   核心步骤：下载Cherry Studio→注册OpenRouter并配置免费阶跃模型stepfun/step-3.5-flash:free→一键安装OpenClaw（需提前装node和git）。

+   OpenClaw是开源个人AI Agent框架，通过SOUL.md/IDENTITY.md/USER.md定义AI身份、性格和主人信息，实现对话记忆与个性化交互。

+   提供作者推荐的8条SOUL配置准则，以及Clawd（小爪）的IDENTITY.md模板、USER.md模板，方便用户快速个性化设置。

+   配置完成重启后即可使用AI龙虾，支持天气查询等功能，隐私性强（支持纯本地使用）。

> 关键词：OpenClaw，Cherry Studio，AI Agent，免费安装，个性化配置

>

> 🔗 文中引用的链接

+   [Cherry Studio下载](https://www.cherry-ai.com/download)

+   [OpenRouter注册](https://openrouter.ai/)

> 4️⃣

> 🤖

> 2026，写给 AI 创业者的慷慨、残酷、与迷雾

>

+   2026年AI coding能力突破，Andrej Karpathy等案例显示AI agent可自主完成复杂编程任务，效率远超人工，Coding Agent能力越过临界点。

+   AI coding商品化遵循历史规律（印刷术、电力、云计算），价值从“代码编写”迁移到“问题定义、产品判断、用户获取”。

+   创业环境残酷：构建门槛降低但竞争指数级上升，注意力成终极瓶颈，赢家通吃，传统技术护城河坍塌。

+   有效策略：避免用AI做旧事，建代码外护城河（数据、用户关系等），重视判断力，拥抱unbundling后找re-bundling机会。

+   AI处于技术革命安装期早期，未来需围绕AI重新设计工作流，“流水线时刻”或在数年到来，耐心和韧性更重要。

> 关键词：AI coding商品化、AI创业策略、价值迁移、技术革命周期、AI agent

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/u9J78jnzjbkjy7CkXt8pKQ)

> 5️⃣

> 🤖

> Lovart用不了？这款国产AI复刻了9...

>

+   国产AI工具星流Agent可作为Lovart平替，国内使用更顺畅，访问xingliu.art即可用，无需特殊门槛。

+   支持单次最高100张图批量生成，配备Touch Edit、文本编辑等与Lovart类似的精准编辑功能。

+   集成多顶尖模型，图像类含自家V2 Flash、Seedream5.0、Midjourney V7等，视频类有可灵3.0等。

+   提供详细元提示词用于批量生成城市主题“她”海报，需遵循严格构图、细节约束。

+   能满足多场景创作，如超酷IP、国风原画、美漫分镜、像素游戏UI等，编辑后可替代PS部分功能。

> 关键词：星流Agent，Lovart平替，AI生图编辑，多模型集成，批量生成

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/xlSx7Aovh6Rvn2kqoQRyMw)

> 6️⃣

> 🤖

> [OpenClaw最强轻量平替负责人领衔！中国生成式](https://mp.weixin.qq.com/s/0euBfSXq2CAXHt2IoZv8Rg)AI大会首批嘉宾揭晓，特设龙虾技术研讨会

>

+   2026年4月21-22日，GenAICon 2026中国生成式AI大会（北京站）将在北京富力万丽酒店举办，主题为“奔赴AGI 重塑未来”，由智东西等联合主办。

+   大会日程含开幕式、3场专题论坛及6场闭门技术研讨会，首次增设OpenClaw技术研讨会（分会场二4月22日上午）。

+   首批嘉宾由OpenClaw最强轻量平替nanobot负责人黄超领衔，涵盖李志宇（大模型记忆）、兰天（Agent记忆框架）等6位学者/专家。

+   同期设人工智能创新展览区，招展火热，覆盖大模型、AI智能体、AIGC应用等产业链方向。

+   报名通道开启，含通票、贵宾票等五类门票，观众票免费需审核，可联系小助手“泡泡”报名。

> 关键词：生成式AI大会、OpenClaw、大模型、AI智能体、技术研讨会

>

> 🔗 文中引用的链接

+   《RLVR新范式！人大&字节提出基于问题拆解的高效强化学习方法，解锁LLM推理新上限》

+   《暴涨28.1k Star！港大开源 OpenClaw 最强轻量平替nanobot，3966 行代码复刻核心能力》

> 🔔AI情报助手早报「03月09日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> Karpathy开启"自动研究"实验：AI开始自己写代码训练自己

>

+   Andrej Karpathy开源autoresearch项目（630行代码），实现AI代理自动完成研究循环：人类写指令→AI改训练代码→跑实验→评估迭代，每小时约12次实验（5分钟/次）。

+   项目核心3文件：prepare.py（固定基础设施）、train.py（AI唯一可修改的训练代码）、program.md（人类研究指令），用git分支管理实验迭代。

+   实测8个H100一晚上跑276次实验，29次有改进，AI还优化随机种子（从经典42改为137）。

+   存在争议：批评者认为是指标驱动的爬山算法（非自主科学研究），支持者认可固定时间窗口强制架构权衡的设计。

+   研究瓶颈转变为“提问题”，技能转向设计研究策略，项目MIT开源（支持NVIDIA GPU，已有macOS fork）。

> 关键词：Karpathy，autoresearch，AI自动研究，机器学习实验，开源项目

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/7u58bVQZAmVwrlA6S-NhtA)

+   autoresearch项目地址

> 2️⃣

> 🤖

> [200万样本、200任务：伯克利、NTU、CMU、斯坦福、约翰](https://mp.weixin.qq.com/s/LHpC12oEzljc7cMz-kGXDQ)霍普金斯等机构联合推进“可验证视频推理”

>

+   多顶尖机构联合提出VBVR套件，以200任务、201.5万图像、100.75万视频构建可训练/验证的视频推理基础设施。

+   VBVR含按5类认知能力设计的数据集（可参数化扩展）及规则可验证评测工具（对齐人类偏好）。

+   微调后的VBVR-Wan2.2基准提升84.6%，多维度超主流专有模型，揭示能力间复杂相关性。

+   数据扩展提升同域/异域泛化，但存在天花板，需架构改造（如状态跟踪、自校正）。

+   强调“可控性先于推理”，模型学会按规则执行，但长时程任务仍有失败模式。

> 关键词：视频推理、VBVR、可验证评测、认知能力、规模化研究

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/LHpC12oEzljc7cMz-kGXDQ)

+   [论文链接](https://huggingface.co/papers/2602.20159)

+   [PaperScope链接](https://www.paperscope.ai/hf/2602.20159)

> 3️⃣

> 🤖

> OpenClaw免费小白安装教程来了！养...

>

+   教程介绍国产开源多模型AI桌面客户端Cherry Studio，支持一键安装OpenClaw，全程免费、零基础可上手，10-20分钟即可拥有持久化AI伙伴🦞。

+   核心步骤：下载Cherry Studio→注册OpenRouter并配置免费阶跃模型stepfun/step-3.5-flash:free→一键安装OpenClaw（需提前装node和git）。

+   OpenClaw是开源个人AI Agent框架，通过SOUL.md/IDENTITY.md/USER.md定义AI身份、性格和主人信息，实现对话记忆与个性化交互。

+   提供作者推荐的8条SOUL配置准则，以及Clawd（小爪）的IDENTITY.md模板、USER.md模板，方便用户快速个性化设置。

+   配置完成重启后即可使用AI龙虾，支持天气查询等功能，隐私性强（支持纯本地使用）。

> 关键词：OpenClaw，Cherry Studio，AI Agent，免费安装，个性化配置

>

> 🔗 文中引用的链接

+   [Cherry Studio下载](https://www.cherry-ai.com/download)

+   [OpenRouter注册](https://openrouter.ai/)

> 4️⃣

> 🤖

> 2026，写给 AI 创业者的慷慨、残酷、与迷雾

>

+   2026年AI coding能力突破，Andrej Karpathy等案例显示AI agent可自主完成复杂编程任务，效率远超人工，Coding Agent能力越过临界点。

+   AI coding商品化遵循历史规律（印刷术、电力、云计算），价值从“代码编写”迁移到“问题定义、产品判断、用户获取”。

+   创业环境残酷：构建门槛降低但竞争指数级上升，注意力成终极瓶颈，赢家通吃，传统技术护城河坍塌。

+   有效策略：避免用AI做旧事，建代码外护城河（数据、用户关系等），重视判断力，拥抱unbundling后找re-bundling机会。

+   AI处于技术革命安装期早期，未来需围绕AI重新设计工作流，“流水线时刻”或在数年到来，耐心和韧性更重要。

> 关键词：AI coding商品化、AI创业策略、价值迁移、技术革命周期、AI agent

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/u9J78jnzjbkjy7CkXt8pKQ)

> 5️⃣

> 🤖

> Lovart用不了？这款国产AI复刻了9...

>

+   国产AI工具星流Agent可作为Lovart平替，国内使用更顺畅，访问xingliu.art即可用，无需特殊门槛。

+   支持单次最高100张图批量生成，配备Touch Edit、文本编辑等与Lovart类似的精准编辑功能。

+   集成多顶尖模型，图像类含自家V2 Flash、Seedream5.0、Midjourney V7等，视频类有可灵3.0等。

+   提供详细元提示词用于批量生成城市主题“她”海报，需遵循严格构图、细节约束。

+   能满足多场景创作，如超酷IP、国风原画、美漫分镜、像素游戏UI等，编辑后可替代PS部分功能。

> 关键词：星流Agent，Lovart平替，AI生图编辑，多模型集成，批量生成

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/xlSx7Aovh6Rvn2kqoQRyMw)

> 6️⃣

> 🤖

> [OpenClaw最强轻量平替负责人领衔！中国生成式](https://mp.weixin.qq.com/s/0euBfSXq2CAXHt2IoZv8Rg)AI大会首批嘉宾揭晓，特设龙虾技术研讨会

>

+   2026年4月21-22日，GenAICon 2026中国生成式AI大会（北京站）将在北京富力万丽酒店举办，主题为“奔赴AGI 重塑未来”，由智东西等联合主办。

+   大会日程含开幕式、3场专题论坛及6场闭门技术研讨会，首次增设OpenClaw技术研讨会（分会场二4月22日上午）。

+   首批嘉宾由OpenClaw最强轻量平替nanobot负责人黄超领衔，涵盖李志宇（大模型记忆）、兰天（Agent记忆框架）等6位学者/专家。

+   同期设人工智能创新展览区，招展火热，覆盖大模型、AI智能体、AIGC应用等产业链方向。

+   报名通道开启，含通票、贵宾票等五类门票，观众票免费需审核，可联系小助手“泡泡”报名。

> 关键词：生成式AI大会、OpenClaw、大模型、AI智能体、技术研讨会

>

> 🔗 文中引用的链接

+   《RLVR新范式！人大&字节提出基于问题拆解的高效强化学习方法，解锁LLM推理新上限》

+   《暴涨28.1k Star！港大开源 OpenClaw 最强轻量平替nanobot，3966 行代码复刻核心能力》