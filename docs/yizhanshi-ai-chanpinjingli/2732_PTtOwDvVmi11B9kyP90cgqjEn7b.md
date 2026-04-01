# 1🈷️09日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Zga6dkDxaox9y5xszygchamanhh](https://v11enp9ok1h.feishu.cn/docx/Zga6dkDxaox9y5xszygchamanhh)

> 🔔AI情报助手早报「01月09日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🎯

> [超棒Claude官方提示词：Anthropic](https://mp.weixin.qq.com/s/HycVhOknpzR3GympyuSMYw)发布Claude 4.x提示工程最佳实践

>

+   Anthropic发布Claude4.x提示工程最佳实践，强调模型从对话者转向执行者，需精确指令而非模糊意图，明确功能边界可激活深层能力。

+   提示需添加上下文背景（如技术限制解释）处理边缘情况，格式控制优先肯定性描述，避免否定性禁止以提升输出专业性。

+   长周期任务需用结构化数据+非结构化笔记做状态管理，新上下文通过读取文件恢复状态而非依赖摘要。

+   工具调用需明确指令区分建议与执行，设定默认行动模式，支持并行处理独立操作及子代理委派阈值。

+   代码开发需清理临时文件、避免过度工程化，前端设计注重独特美学减少AI味，高阶认知需结构化研究（竞争性假设+自我批评）。

> 关键词：Claude4.x，提示工程，状态管理，工具调用，代码规范

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/HycVhOknpzR3GympyuSMYw)

+   [参考资料](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices)

> 2️⃣

> 🎯

> [A](https://mp.weixin.qq.com/s/OO5nhjX-1zTSXBzQqH5IQA)nthropic 社区负责人连更31条Claude Code技巧！比Claude Code创始人私藏的还硬核

>

+   Anthropic社区负责人Ado Kukic连续31天分享Claude Code技巧，整理成体系化指南"Advent of Claude 2025"，覆盖从基础到专业级能力。

+   基础命令/init可扫描代码库生成CLAUDE.md文档，含项目构建、测试等信息，支持自定义模块规则。

+   快捷键如!前缀执行bash命令、@Mentions引用上下文，大幅提升操作效率。

+   会话管理支持--continue断点续传、--teleport云端同步多端会话，避免上下文丢失。

+   高级功能包括Plan Mode规划、Subagents并行协作、Hooks干预流程，助力自动化与复杂任务处理。

> 关键词：Claude Code，使用技巧，Anthropic，会话管理，高级功能

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/OO5nhjX-1zTSXBzQqH5IQA)

+   Claude Code Web端

+   Claude Code Chrome扩展

+   Claude Agent SDK

+   （推测npm链接，原文提及SDK导入）

> 3️⃣

> 🔥 [Claude Code最强开源对手！Gi...] (

> [https://mp.weixin.qq.com/s/RZD](https://mp.weixin.qq.com/s/RZDL6UBIfTzICX8GwqSzYw))L6UBIfTzICX8GwqSzYw)

>

+   开源编程工具OpenCode在GitHub获50.2K Star，是Claude Code的开源替代，支持免费使用GLM4.7、MiniMax2.1等模型，无需登录即可白嫖。

+   OpenCode配合插件oh-my-opencode更强大，集成Claude Code与AmpCode核心优势，支持几乎所有主流AI模型（含Gemini、ChatGPT等，可API或账号登录）。

+   OpenCode提供终端TUI（赛博朋克风格可视化）和桌面端两种形态，桌面端傻瓜式安装，终端安装需Node.js环境。

+   Oh-my-opencode插件具备异步SubAgent分身术、关键词触发模式、LSP&AST代码结构理解、上下文自动压缩、防代码截断等实用功能。

+   OpenCode可通过npm或curl安装，oh-my-opencode需Node.js和Bun运行时，安装后配置模型订阅情况即可使用。

> 关键词：OpenCode，oh-my-opencode，开源AI编程工具，Claude Code替代，多模型支持

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/RZDL6UBIfTzICX8GwqSzYw)

+   [OpenCode下载地址](https://opencode.ai/download)

+   oh-my-opencode GitHub仓库

+   [OpenCode安装脚本](https://opencode.ai/install)

+   [Bun安装地址](https://bun.sh/install)

> 4️⃣

> 🪄

> [150k数据反超Qwen-2509！支持10图输入](https://mp.weixin.qq.com/s/Bnhqhy6ojYyU2h5FRvpEtg)，MICo-150k刷新多图融合SOTA

>

+   多图融合任务因现有数据集分割不充分、生成同质化、场景单一等问题受限，缺乏高质量开源数据支撑。

+   香港理工等团队构建MICo-150K数据集，含15万+样本、7子任务27细粒度类型，采用Composed-by-Retrieval策略和自动验证机制保证质量。

+   BLIP-3o、Qwen-Image等不同架构模型在MICo-150K训练后性能显著提升，验证数据集稳健性。

+   基于该数据集训练的Qwen-MICo模型，以150k数据反超更大数据量的Qwen-Image-2509，且支持任意数量图像输入。

+   论文还推出MICo-Bench基准套件和Weighted-Ref-VIEScore定制指标，完善多图融合任务评估体系。

> 关键词：MICo-150K，多图融合，Qwen-MICo，数据集，图像生成

>

> 🔗 文中引用的链接

+   [项目主页](https://mico-150k.github.io/)

+   [GitHub](https://github.com/A113N-W3I/MICo-150K)

+   [Online Demo](https://huggingface.co/spaces/kr-cen/Qwen-Image-MICo)

+   [Arxiv链接](https://arxiv.org/abs/2512.07348)

+   [原文章链接](https://mp.weixin.qq.com/s/Bnhqhy6ojYyU2h5FRvpEtg)

> 5️⃣

> 🌱

> 除了写代码，Claude 还能……种菜

>

+   开发者Martin DeVido搭建生物舱，让Claude24/7全天候运行、全权照顾番茄苗Sol（无人类干预），通过Arduino连接传感器与设备形成反馈循环，每15-30分钟监控状态。

+   Day34因Arduino递归错误导致系统崩溃，Claude分诊处理（开生长灯/加热垫/排风扇、浇水600ml），13分钟后让Sol恢复生机。

+   Claude解决湿度偏高致叶尖发黄问题，目前Sol处于营养生长期，已长15-20片叶子，枝繁叶茂。

+   Martin强调不鼓励购买相关meme coin（销毁收到的代币），建议通过其网站购买AI肖像画支持项目（覆盖电费等成本）。

+   用户可通过直播地址实时观看Claude照顾Sol，或访问项目商店支持。

> 关键词：Claude，AI种菜，生物舱，反馈循环，系统危机处理

>

> 🔗 文中引用的链接

+   [直播地址](https://autoncorp.com/biodome)

+   [项目商店](https://autoncorp.com/store)

> 6️⃣

> 🎯

> 马斯克都爱用的Grok定时任务，治好了我...

>

+   作者尝试多种复杂信息流方法后，发现Grok定时任务轻量方案，无需编程即可筛选X平台高赞高互动帖子（大佬账号或特定关键词）。

+   通过优化Grok提示语（指定时间/热度/格式），作者解决初期搜索质量问题，获取精准的每日热帖监控结果。

+   利用Grok Task定时功能推送指定账号列表总结，减少信息焦虑，获得连续思考时间。

+   该方法可扩展到元宝（公众号）和Manus（多Agent），实现多平台监控，甚至结合Genspark抓资源。

+   定时日报折叠信息不确定性，成为带时间戳的笔记薄片，方便选题案例收集，避免信息坟场。

> 关键词：Grok定时任务，信息焦虑，高赞帖子筛选，多平台监控，定时日报

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/YEX9kMgmQjKqzokyXl1tWg)

+   [X账号列表链接](http://x.com/i/lists/1695376776867062037)

+   [Grok任务页面链接](http://grok.com/tasks)