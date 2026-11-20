
# 9🈷️12日

来源：<https://v11enp9ok1h.feishu.cn/docx/UnWXd4O5toVTXBxFWipcbK4aned>

🔔AI情报助手午报「09月12日」

## 1️⃣ Vibe Coding之模型思维（一）

🎯

文章探讨Vibe Coding中如何运用模型思维，让AI更好融入开发流程。

介绍6A工作流和5S敏捷开发规则，提升AI开发效率与质量。

强调模型思维需在项目架构初为AI设“参与点”，建立质量度量与反馈机制。

给出7个技巧，如给AI “工牌身份”、翻译“需求”等，让AI开发更规范高效。

模型思维是将项目开发模型与AI能力融合，把控质量与业务价值。

关键词：Vibe Coding，模型思维，6A工作流，5S敏捷，AI开发

🔗 文中引用的链接

+ <https://newsletter.pragmaticengineer.com/p/vibe-coding-as-a-software-engineer?triedRedirect=true>
+ <https://developer.volcengine.com/articles/7537170173321543699>
+ innovation.world
+ <https://sites.lsa.umich.edu/scottepage/>

## 2️⃣ 0用户0产品估值850亿，ChatGPT...

🎯

“ChatGPT之母”创办的Thinking Machines Lab发布论文《克服LLM推理中的不确定性》，挑战大模型对话不确定性难题。

团队质疑“并发 + 浮点”主流假说，认为问题源于批次不变性缺失。

提出通过固定计算内核策略解决问题，并展示对Transformer三个核心模组的改造。

实验表明改造后可实现100%生成结果可复现性，代价是牺牲约13.5%的峰值吞吐量。

该研究为强化学习等技术清除障碍，呼唤工程严谨性回归。

关键词：LLM，推理不确定性，批次不变性，Transformer，确定性改造

🔗 文中引用的链接

+ [博客链接](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/)

## 3️⃣ 一夜之间，Claude的口碑崩了

🤖

因CEO反华言论及模型降智，Claude口碑崩塌，大量用户退订转投CodeX。

自7月底起，Claude被投诉过度附和用户，8月中旬开始，开发者反馈其出现使用限制收紧、延迟错误等“降智”问题。

8月下旬问题达到峰值，Anthropic官方承认质量降级并表示已修复。

9月10日，Anthropic称输出质量不稳定，部分已修复，仍在调查Opus 4.1降质问题。

同时，Claude限制用户使用上限、禁止中资企业使用，引发开发者不满。

关键词：Claude，模型降智，用户流失，Anthropic，AI编程工具

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/AwSrZArmF8bxmQxGMEHovA)

## 4️⃣ 腾讯悄悄上线了“Claude Code”...

🤖

腾讯悄悄上线“Claude Code”（即CodeBuddy Code），加入命令行编程Agent战场，定位为AI CLI ，全面开放无需邀请码。

作者使用CodeBuddy Code搭建MCP服务，实现生成、编辑图片等功能，体验到其开发速度快、操作便捷等优点，但也发现稳定性问题。

CodeBuddy Code使用简单，安装Node.js后通过命令安装，登录可选国内国外版本，还能切换模型。

编程Agent是热门领域，腾讯持续发力，CodeBuddy不断带来惊喜。

作者希望CodeBuddy继续优化，鼓励读者尝试。

关键词：腾讯，Claude Code，编程Agent，AI CLI，CodeBuddy

🔗 文中引用的链接

+ <https://mp.weixin.qq.com/s/u4rx_fUdRr8l3cuCcfyTEg>
+ <https://nodejs.org/zh-cn/download>
+ <https://ai.google.dev/gemini-api/docs/image-generation?hl=zh-cn#python>

## 5️⃣ OpenAI 最新研究：AI 模型产生“幻觉”，竟是被我们“教坏了”

🤖

OpenAI研究指出，AI幻觉非模型Bug，而是“合理但错了”，源于训练目标不鼓励诚实及评估机制逼模型乱猜。

真实案例显示，越“诚实”模型因评估只看准确率，得分反而更低。

降低幻觉需优化评估机制，鼓励表达不确定，设定信心阈值引导模型自我判断。

同时，研究澄清四大幻觉误区，强调改变评估体系重要性。

幻觉因规则鼓励猜测，优化打分标准才能让AI“更靠谱”。

关键词：AI幻觉，OpenAI，评估机制，训练目标，大模型

🔗 文中引用的链接

+ Why Language Models Hallucinate
