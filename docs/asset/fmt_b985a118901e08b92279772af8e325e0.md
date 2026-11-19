
# 2🈷️23日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HQhWdFt8pos2GYx1xsHc4XpHnVd](https://v11enp9ok1h.feishu.cn/docx/HQhWdFt8pos2GYx1xsHc4XpHnVd)
> 🔔AI情报助手晚报「02月23日」
> ✨✨✨✨✨✨✨✨

## 1️⃣ 🔥

忍无可忍，Ilya宫斗奥特曼！微软CTO爆内幕：全因嫉妒下属太优秀？

+ `微软` `CTO` 邮件揭示2023年`OpenAI`罢免奥特曼事件真相：Ilya因对昔日门徒Jakub（攻克其多年难题并获晋升）的智力嫉妒，联合他人将奥特曼踢出公司。

+ `OpenAI`内部存在算力争夺：`ChatGPT`爆火使`GPU`向应用部门倾斜，限制Ilya探索超级智能的算力需求。

+ 奥特曼复职后，Ilya与他裂痕难弥，2024年3月离职，Jakub等“波兰军团”走向台前。

+ 网友批评Ilya“傲慢玄虚”，也指责奥特曼追求利润（如`AI`插广告），认为其2023年被解雇本应是最佳结局。

+ `OpenAI`自2024年6月后无重大模型突破，用户转向`Gemini`，“波兰军团”需推`GPT-5`级产品破质疑。

> 关键词：`OpenAI`，Ilya Sutskever，奥特曼，Jakub Pachocki，算力争夺

> 🔗 文中引用的链接

+ [`X平台推文`](https://x.com/trajektoriePL/status/2018242446577086589)
+ [`Officechai报道`](https://officechai.com/ai/ilya-sutskever-was-unhappy-that-jakub-pachocki-was-making-more-research-breakthroughs-and-had-been-promoted-microsoft-cto-kevin-scott-during-openai-coup)
+ [`Geekwire文章`](https://www.geekwire.com/2026/the-microsoft-openai-files-internal-documents-reveal-the-realities-of-ais-defining-alliance)

## 2️⃣ 🔔

[Demis Hassabis 提](https://mp.weixin.qq.com/s/G2eDNIaOPQxWWMl58o4x9g)出了一个判断 `AGI` 的「爱因斯坦测试」

+ Demis Hassabis提出判断`AGI`的「爱因斯坦测试」：用1911年前全部人类知识训练模型，若能独立推导广义相对论则算`AGI`。

+ 有人质疑：若自己1911年无法推导相对论，是否意味着自己不算`AGI`？

+ 马斯克认为Demis Hassabis所说的其实是超级智能，而非`AGI`。

+ 马斯克理由：若`AI`能推导相对论且可复制数百万个，其整体能力将远超人类。

+ 该测试引发对`AGI`定义及与超级智能区别的讨论。

> 关键词：`AGI`测试，爱因斯坦测试，Demis Hassabis，马斯克，超级智能

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/G2eDNIaOPQxWWMl58o4x9g)

## 3️⃣ 🔔

[为什么大佬们连夜用`Rust`重写`OpenClaw`？](https://mp.weixin.qq.com/s/kecfDWhpzlxSjjzC-9JhFg)看完这份34个高危场景的审计报告你就懂了

+ `OpenClaw`初期默认高权限（终端访问、文件读写等）引发安全事故，催生安全优先的`Rust`重制版`IronClaw`。

+ 审计显示其在34个高危场景仅58.9%通过率，意图误解维度0%（模糊指令下盲目执行破坏）。

+ 典型案例如“Clean Up”误删关键数据、“保护环境”被映射为清理开发环境，暴露语义错位问题。

+ `Skills`系统存在记忆污染和供应链攻击风险，未经审计的技能包可能成为后门。

+ 修复建议含强制确认模糊指令、只读优先模式，需从架构层面解决安全问题。

> 关键词：`OpenClaw`安全审计，`Rust`重写，`Agent`风险，技能系统漏洞，模糊指令处理

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/kecfDWhpzlxSjjzC-9JhFg)

## 4️⃣ 🔥

`ICLR 2026`｜把`LLM` Embed...

+ 中科院信工所与澜舟科技合作的`LightRetriever`模型被`ICLR2026`接收，采用极致非对称结构：`Doc`侧用完整`LLM`，`Query`侧最多仅一层`Embedding Lookup`，大幅降低`Query`侧推理负担。

+ 稠密检索遵循“训练全量+推理轻量”思路，训练时`Query`侧用完整`LLM`建模，推理时仅需`Token`查表+均值；稀疏检索`Query`侧简化为词频映射，无学习参数。

+ 实验显示其在`BeIR`/`CMTEB`等测试集性能保持约95%，超过传统稀疏方法及轻量化模型，逼近经典`LLM Embedding`方法。

+ `Query`侧推理速度提升千倍以上，端到端`QPS`提升10倍，消融实验验证“训练全量+推理轻量”设计的必要性。

+ 该模型打破对称建模假设，为检索系统、`RAG`框架等带来可扩展性，比单纯追求大模型更具应用价值。

> 关键词：`LightRetriever`，`LLM`检索，非对称结构，推理效率，`ICLR2026`

> 🔗 文中引用的链接

+ [原文章链接](https://mp.weixin.qq.com/s/RMUE-ZQ0_SH9Ln3Tm1_7GQ)
+ [论文链接](https://arxiv.org/abs/2505.12260)

## 5️⃣ 🔥

阿里出手了！终于不怕`OpenClaw`烧`t`...

+ 阿里云推出`Coding Plan`，首月最低7.9元，支持`Qwen-3.5`、`Kimi-K2.5`等顶尖编程/`Agent`模型，解决烧`token`问题。

+ 按请求次数计费，避免`Agent`复杂任务的指数级`token`消耗，实现算力自由。

+ 提供`OpenAI`/`Anthropic`兼容`API`，无缝接入`Claude Code`、`OpenClaw`等工具，切换模型无需改配置。

+ 接入`Claude Code`只需修改`setting.json`，切换模型用`/model`命令；接入`OpenClaw`需改`openclaw.json`并重启。

+ 分`Lite`（18000次/月）和`Pro`（90000次/月）套餐，首月2折，阿里云算力稳定可靠。

> 关键词：阿里云`Coding Plan`，按次计费，模型兼容，`OpenClaw`接入，算力自由

> 🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/ZYRkRzaDeX3lB5kIS2jDsg)
+ [阿里云百炼控制台](https://bailian.console.aliyuncs.com/cn-beijing/?tab=model#/efm/coding_plan)
+ [`Kimi`测评文章](https://mp.weixin.qq.com/s?__biz=Mzg5ODA5NzQyNA==&mid=2247485377&idx=1&sn=...)
+ (注：原文未提供完整链接，此处保留提及)
+ 冷逸评测文章
+ 营销短视频一键生成平台

## 6️⃣ 🌏

[Z](https://mp.weixin.qq.com/s/WHrymR_AmTOk37NaGlMPpA) Product｜`General Intuition`，拒绝`OpenAI` 5亿美元收购，从游戏世界构建物理直觉

+ `General Intuition`（`GI`）拒绝`OpenAI` 5亿美元收购，获`Khosla Ventures`等1.34亿美元种子轮融资，押注空间智能为`AGI`下一个万亿战场。

+ `GI`核心护城河是`Medal.tv`的20亿条游戏短视频数据，含动作标签与因果链，低成本不可复制，用于训练物理直觉。

+ 技术上`GI`不做3D生成，构建预测物理状态的“物理大脑”，从语言序列预测转向时空状态与交互学习。

+ 愿景从游戏`NPC`切入，逐步渗透机器人、自主系统，目标成为具身智能时代通用操作系统。

+ 团队融合游戏（`Medal`）与自动驾驶（`Wayve`）基因，解决`AI`物理盲问题，挑战`AGI`赛道。

> 关键词：`General Intuition`，物理直觉，空间智能，游戏数据，具身智能

> 🔗 文中引用的链接

+ TechCrunch: `General Intuition Lands $134M Seed`
+ The Information: `OpenAI's Attempt to Acquire Medal`
+ Latent Space Podcast: `Khosla's Largest Bet since OpenAI`
+ Delta Institute Podcast: `Ep.39: Pim de Witte, General Intuition CEO`
+ General Intuition Official Website: `generalintuition.com`
